<template>
  <v-content>
    <v-flex xs1 md2></v-flex>
    <c-upload @upload="upload" @watchMore="openDialog" :uploadWidth="width" @add="addFiles">
      <template v-slot:images>
        <c-image 
          @watch="watch"
          @close="delFiles"
          :imgSrc="settings.imgSrc"
          :height="settings.height"
        ></c-image>
      </template>
    </c-upload>

    <c-dialog 
      @close="closeDialog" 
      :showDialog="dialogsSettings.showDialog" 
      :width="dialogsSettings.width"
      :display="dialogsSettings.display"
    >
      <template v-slot:images>
        <c-image 
          @watch="watch"
          @closeImg="closeImg"
          @close="delFiles" 
          :wraps="dialogsImagesSetting.wraps"
          :minWidth="dialogsImagesSetting.minWidth"
          :maxWidth="dialogsImagesSetting.maxWidth"
          :watchTarget="dialogsImagesSetting.watchTarget"
          :closeTarget="dialogsImagesSetting.closeTarget"
          :imgSrc="dialogsImagesSetting.imgSrc"
          :height="dialogsImagesSetting.height"
          :contain="dialogsImagesSetting.contain"
          :margin="dialogsImagesSetting.margin"
        ></c-image>
      </template>
    </c-dialog>
  </v-content>
</template>

<script>
import Upload from "../components/Upload"
import Dialogs from "../components/Dialogs"
import Images from "../components/Images"

export default {
  name: 'VUploader',
  components: {
    "c-upload": Upload,
    "c-dialog": Dialogs,
    "c-image": Images
  },
  props: {
    width: {
      type: String
    },
    initial: {
      type: Array,
      default: []
    }
  },
  data: () => ({
    settings: {
      imgSrc: [],
      height: 120
    },
    dialogsSettings: {
      showDialog: false,
      width: 800,
      display: "flex"
    },
    dialogsImagesSetting: {
      wraps: true,
      minWidth: 190,
      maxWidth: 190,
      watchTarget: "watch",
      closeTarget: "close",
      imgSrc: [],
      height: 150,
      contain: false,
      margin: 1
    },
    upLoadFiles: []
  }),
  created() {
    let fileName
    let fileType
    let imgSrcs
    
    for(let i = 0; i < this.initial.length; i++) {
      fileName = (this.initial[i].split("/")[this.initial[i].split("/").length - 1])
      fileType = fileName.split(".")[1]
      
      if(fileType !== undefined) {
        let re = new RegExp(".(jpeg|jpg|gif|png|apng|svg|bmp)", "g")
        let result = re.exec(fileName)

        imgSrcs = { 
          imgName: fileName, 
          src: this.initial[i], 
          show: true, 
          isLocal: false
        }
        
        if(result !== null) {
          imgSrcs.fileType = "image"
        }else {
          imgSrcs.fileType = fileType
        }
        this.settings.imgSrc.push(imgSrcs)
      }

      imgSrcs = null
      fileName = null
      fileType = null
    }


  },
  methods: {
    openDialog() {
      this.dialogsSettings.width = (190 + 8) * 4
      this.dialogsSettings.showDialog = true

      this.dialogsImagesSetting.height = 150

      this.setImgSrc("watchMore", null)
    },
    closeDialog() {
      this.dialogsSettings.showDialog = false
      this.dialogsImagesSetting.imgSrc = []

      this.dialogsImagesSetting.contain = false
    },
    watch(index) {
      this.dialogsSettings.showDialog = true
      this.dialogsSettings.width = 1200

      this.dialogsImagesSetting.height = 750

      this.setImgSrc("watch", index)
    },
    setImgSrc(type, index) {
      if(type === "watchMore") {
        this.dialogsSettings.display = "flex"

        this.dialogsImagesSetting.imgSrc = this.settings.imgSrc
        this.dialogsImagesSetting.minWidth = 190
        this.dialogsImagesSetting.maxWidth = 190
        this.dialogsImagesSetting.margin = 1
      }else if(type === "watch") {
        this.dialogsImagesSetting.contain = true

        this.dialogsSettings.display = "none"

        this.dialogsImagesSetting.imgSrc = [this.settings.imgSrc[index]]
        this.dialogsImagesSetting.watchTarget = ""
        this.dialogsImagesSetting.closeTarget = "closeImg" 
        this.dialogsImagesSetting.minWidth = 1200
        this.dialogsImagesSetting.maxWidth = 1200
        this.dialogsImagesSetting.margin = 0
      }
    },
    addFiles(files) {
      // this.upLoadFiles = files
      this.$emit("add", this.upLoadFiles)
    },
    delFiles(delData) {
      if(typeof delData === "number") {
        this.upLoadFiles.splice(delData, 1)
      }else {
        this.$emit("delete", delData)
      }
    },
    closeImg() {
      this.closeDialog()
    },
    upload(imgSrcs, files) {
      this.upLoadFiles = files
      this.settings.imgSrc.push(imgSrcs)
    }
  }
}
</script>

<style>

</style>
