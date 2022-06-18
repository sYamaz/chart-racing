<template>
  <v-card>
    <v-responsive :aspect-ratio="data.aspectRatio">
      <Chart :data="data.chartData"></Chart>
    </v-responsive>
  </v-card>
</template>

<script lang="ts">
import {Vue, Component, Prop} from "nuxt-property-decorator"
import Chart, {ChartData} from "~/components/atoms/Chart.vue"


export interface GraphAreaData{
  chartData:ChartData
  aspectRatio:number
}

@Component({components:{
  Chart
}})
export default class GraphArea extends Vue{
  @Prop()
  data!: GraphAreaData
}
</script>

<style>

</style>
