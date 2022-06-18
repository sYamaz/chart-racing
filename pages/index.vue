<template>
  <MainPageTemplate :data="pagedata"></MainPageTemplate>
</template>

<script lang="ts">
import { Vue, Component, Provide } from 'nuxt-property-decorator'
import { THEME_KEY } from 'vue-echarts'
import { ChartValue } from '~/components/atoms/Chart.vue'
import MainPageTemplate, {
  MainPageTemplateData,
} from '~/components/templates/MainPageTemplate.vue'

interface sliceDatas {
  slice: {
    sliceTitle: string
    datas: {
      label: string
      value: number
    }[]
  }[]
}

@Component({
  components: {
    MainPageTemplate,
  },
})
export default class IndexPage extends Vue {
  @Provide(THEME_KEY)
  private mode: string = 'light'

  private processing: boolean = false

  get pagedata(): MainPageTemplateData {
    return {
      controlData: {
        startButtonData: {
          onClick: this.start,
        },
        fileSelectorData: {
          fileSelected: this.loadTsv,
        },
        stopButtonData: {
          onClick: this.stop,
        },
      },
      graphData: {
        aspectRatio: 16 / 9,
        chartData: {
          title: this.title,
          titleColor: this.titleColor,
          maxDisplayCount: 5,
          samplingSpan: 300,
          values: this.values,
          option: {
            colorTable: this.colorTable,
          },
        },
      },
    }
  }

  private title: string = 'Title'
  private titleColor: string = '#FFFFFF'
  private values: ChartValue[] = []
  private colorTable: { label: string; color: string }[] = []

  private sliceData: sliceDatas = {
    slice: [],
  }

  private stopRequired: boolean = false

  private stop() {
    this.stopRequired = true
  }

  private async start() {
    const span = this.pagedata.graphData.chartData.samplingSpan

    for (let i = 0; i < this.sliceData.slice.length; i++) {
      const slice = this.sliceData.slice[i]

      this.title = slice.sliceTitle
      this.values = slice.datas.map((d) => {
        return {
          label: d.label,
          value: d.value,
        }
      })

      await new Promise((resolve) => setTimeout(resolve, span))

      if (this.stopRequired) {
        this.stopRequired = false
        this.values = []
        this.title = ''
        break
      }
    }
  }

  private loadTsv(file: File) {
    if (file === null) {
      return
    }

    this.processing = true

    file.text().then((t) => {
      const labels: { label: string; index: number }[] = []
      const datas: sliceDatas = {
        slice: [],
      }

      const lines = t.split('\n')
      // 1行目
      const headerline = lines[0].split('\t')
      const sliceNameColIndex = headerline.findIndex(
        (txt) => txt.toLowerCase() === 'slicename'
      )
      if (sliceNameColIndex < 0) {
        // eslint-disable-next-line no-console
        console.error('sliceName列がありません')
      }
      headerline.forEach((txt, i) => {
        if (i !== sliceNameColIndex) {
          labels.push({
            label: txt,
            index: i,
          })
        }
      })
      // 2行目
      const colorLine = lines[1].split('\t')
      const colors: { label: string; color: string }[] = []
      this.titleColor = colorLine[sliceNameColIndex]
      labels.forEach((l) => {
        const color =
          colorLine.length > l.index ? colorLine[l.index] : '#808080'
        colors.push({
          label: l.label,
          color,
        })
      })
      this.colorTable = colors

      lines
        .slice(2)
        .map((dataLine) => dataLine.split('\t'))
        .forEach((line) => {
          const title =
            line.length > sliceNameColIndex ? line[sliceNameColIndex] : ''

          const slice: {
            sliceTitle: string
            datas: {
              label: string
              value: number
            }[]
          } = {
            sliceTitle: title,
            datas: [],
          }

          labels.forEach((l) => {
            if (line.length > l.index) {
              if (Number.isNaN(line[l.index])) {
                return
              }
              slice.datas.push({
                label: l.label,
                value: Number(line[l.index]),
              })
            }
          })

          datas.slice.push(slice)
        })

      this.sliceData = datas
      this.processing = false
    })
  }
}
</script>
