<template>
  <v-container>
    <v-card :width="uploadWidth" color="blue-grey lighten-5" elevation="6">
        <v-card flat height="150px" style="overflow: auto; overflow-y: hidden">
          <v-flex class="pa-1" >
            <slot name="images"></slot>
          </v-flex>
        </v-card>

        <v-footer height="auto" color="blue-grey lighten-4">
          <v-card-actions>
            <v-btn type="file" @click="select" @change="changeImg" color="info">选择文件</v-btn>
            <v-btn type="file" @click="upload" color="info">上传<v-icon right dark>cloud_upload</v-icon></v-btn>
            <input type="file" style="display: none;" multiple="multiple" ref="select" @change="changeImg">
          </v-card-actions>

          <v-spacer></v-spacer>

          <v-card-actions>
            <v-btn color="info" @click="watch">更多</v-btn>
          </v-card-actions>
        </v-footer>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    props: {
      uploadWidth: {
        type: String,
        default: "600"
      },
      settings: {
        type: Object
      }
    },
    data: () => ({
      upLoadFiles: []
    }),
    methods: {
      watch() {
        this.$emit("watchMore")
      },
      select () {
        let selectbtn = this.$refs.select
        selectbtn.click()
      },
      changeImg (event) {
        let files = event.currentTarget.files
        this.getReader("upload", files)
      },
      upload() {
        this.$emit("add", this.upLoadFiles)
      },
      getReader(eventTarget, files) {
        let imgSrcs
        for(let i = 0; i < files.length; i++) {
          this.upLoadFiles.push(files[i])

          let reader = new FileReader()
          reader.readAsDataURL(files[i])
          reader.onload = (event) => {
            imgSrcs = { 
              imgName: files[i].name, 
              fileType: files[i].type.split("/")[0], 
              src: event.target.result,
              isLocal: true
            }
            if (files[i].type.split("/")[0] === "image") {
              imgSrcs.show = false
            }else {
              imgSrcs.show = true
            }
            if(eventTarget === "upload") {
              this.$emit(eventTarget, imgSrcs, this.upLoadFiles)
              imgSrcs = null
            }
          }
          reader = null
        }
      }
    }
  }
</script>

<style>
</style>
