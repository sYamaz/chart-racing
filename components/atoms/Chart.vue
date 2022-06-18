<template>
  <VChart class="chart" :option="option" autoresize></VChart>
</template>

<script lang="ts">
import VChart from 'vue-echarts'
import {Vue, Component, Prop} from 'nuxt-property-decorator'
import { CanvasRenderer } from 'echarts/renderers'
import { BarChart } from 'echarts/charts'
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  GraphicComponent,
} from 'echarts/components'
import {use} from 'echarts/core'
use([
  CanvasRenderer,
  BarChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  GraphicComponent
])

export interface ChartValue{
  label: string,
  value: number
}

export interface ChartOption{
  colorTable:{label:string, color:string}[]
}

export interface ChartData{
  title: string
  titleColor:string
  maxDisplayCount:number
  samplingSpan: number
  values: ChartValue[]
  option:ChartOption
}

@Component({
  components:{VChart}
})
export default class Chart extends Vue {

  @Prop()
  data!:ChartData


  get option() {
    return {
        xAxis: {
          max: 'dataMax',
          axisLabel: {
            formatter: (n:any) => {
              return n + ''
            }
          }
        },
        yAxis: {
          type: 'category',
          inverse: true,
          max: this.data.maxDisplayCount - 1, // only the largest 3 bars will be displayed
          data: this.data.values.slice().map(d => d.label),
          axisLabel: {
            show:true,
            fontSize: 14,
            formatter: (v:any) => {
              return v
            },
            // rich: {
            //   flag: {
            //     fontSize: 25,
            //     padding: 5
            //   }
            // }
          },
          animationDuration: this.data.samplingSpan,
          animationDurationUpdate: this.data.samplingSpan,
        },
        series: [
          {
            realtimeSort: true,
            name: 'X',
            type: 'bar',
            data: this.data.values.slice().map(d => d.value),
            itemStyle: {
              color: (param:any) => {
                const idx = this.data.option.colorTable.findIndex(o => o.label === param.name)
                if(idx >= 0){
                  return this.data.option.colorTable[idx].color
                }
                return param.color
              }
            },
            label: {
              show: true,
              position: 'right',
              valueAnimation: true
            }
          }
        ],
        legend: {
          show: false
        },
        animationDuration: 0,
        animationDurationUpdate: 3000,
        animationEasing: 'linear',
        animationEasingUpdate: 'linear',
        graphic: {
          elements: [
          {
            type: 'text',
            right: 160,
            bottom: 60,
            style: {
              text: this.data.title,
              font: 'bolder 4rem monospace',
              fill: this.data.titleColor
            },
            z: 100
          }
      ]
    }
    }
  }
}
</script>

<style>

</style>
