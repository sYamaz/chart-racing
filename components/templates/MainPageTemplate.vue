<template>
  <v-container fluid>
    <GraphArea class="ma-1" :data="data.graphData"></GraphArea>
    <ControlArea class="ma-1" :data="data.controlData"></ControlArea>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'nuxt-property-decorator'
import ControlArea, {
  ControlAreaData,
} from '~/components/organisms/ControlArea.vue'
import GraphArea, { GraphAreaData } from '~/components/organisms/GraphArea.vue'

export interface MainPageTemplateData {
  graphData: GraphAreaData
  controlData: ControlAreaData
}

@Component({
  components: {
    ControlArea,
    GraphArea,
  },
})
export default class MainPageTemplate extends Vue {
  @Prop()
  data!: MainPageTemplateData
}
</script>

<style></style>
