<template>
  <v-card class="pa-2">
    <FileSelector :data="data.fileSelectorData"></FileSelector>
    <v-card-actions>
      <v-spacer />
      <start-button :data="data.startButtonData"></start-button>
      <StopButton :data="data.stopButtonData"></StopButton>
      <v-spacer />
    </v-card-actions>
  </v-card>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'nuxt-property-decorator'
import StartButton, {
  StartButtonData,
} from '~/components/atoms/StartButton.vue'
import FileSelector, {
  FileSelectorData,
} from '~/components/atoms/FileSelector.vue'
import StopButton, { StopButtonData } from '~/components/atoms/StopButton.vue'

export interface ControlAreaData {
  startButtonData: StartButtonData
  fileSelectorData: FileSelectorData
  stopButtonData: StopButtonData
}

@Component({
  components: {
    StartButton,
    FileSelector,
    StopButton,
  },
})
export default class ControlArea extends Vue {
  @Prop()
  data!: ControlAreaData
}
</script>

<style></style>
