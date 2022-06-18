<template>
  <v-btn @click="onClick">
    <v-icon> mdi-play </v-icon>
    Start
  </v-btn>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'nuxt-property-decorator'

export interface StartButtonData {
  onClick: () => void
}

@Component({ components: {} })
export default class StartButton extends Vue {
  @Prop()
  data!: StartButtonData

  onClick() {
    this.data.onClick()
  }
}
</script>

<style></style>
