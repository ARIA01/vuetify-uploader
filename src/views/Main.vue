<template>
  <v-content>
    <v-uploader 
      :initial="initial"
      width="600"
      @add="addfile"
      @delete="deletefile"
    ></v-uploader>
  </v-content>
</template>

<script>
import VUploader from "../components/VUploader"

export default {
  name: 'Main',
  components: {
    "v-uploader": VUploader
  },
  data: () => ({
    initial: [
      "https://unsplash.it/150/300?image=1",
      "https://unsplash.it/150/300?image=2",
      "https://unsplash.it/150/300?image=3",
      "https://unsplash.it/150/300?image=4",
      "https://unsplash.it/150/300?image=5",
      "https://unsplash.it/150/300?image=6",
      "https://unsplash.it/150/300?image=7",
      "https://unsplash.it/150/300?image=8",
      "https://unsplash.it/150/300?image=9",
      "http://soft.zdfans.wang/IDM_575.zip",
      "http://soft.zdfans.wang/IDM_575.zip",
      "http://soft.zdfans.wang/IDM_575.zip"
    ]
  }),
  methods: {
    addfile(files) {
      console.log("addfile", files)
    },
    deletefile(fileArray) {
      console.log("deletefile", fileArray)
    }
  }
}
</script>

<style>

</style>
