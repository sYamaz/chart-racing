<template>
  <v-btn @click="onClick">
    <v-icon>mdi-stop</v-icon>
    Stop
  </v-btn>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "nuxt-property-decorator";

export interface StopButtonData{
  onClick:() => void
}

@Component({components:{

}})
export default class StopButton extends Vue {
  @Prop()
  data!: StopButtonData

  onClick(){
    this.data.onClick()
  }
}
</script>

<style>

</style>
