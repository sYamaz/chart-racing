<template>
  <v-file-input v-model="file" placeholder="Upload TSV file" prepend-icon="mdi-paperclip"></v-file-input>
</template>

<script lang="ts">
import Vue from "vue";
import {Component, Prop, Watch} from "nuxt-property-decorator";

export interface FileSelectorData{
  fileSelected: (file:File) => void
}

@Component
export default class FileSelector extends Vue {
  file:File | null = null

  @Watch('file')
  onFilesSelected(){
    this.data.fileSelected(this.file!)
  }

  @Prop()
  data!:FileSelectorData
}
</script>

<style>

</style>
