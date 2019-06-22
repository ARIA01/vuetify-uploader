<template>
<v-content>
  <v-layout row :wrap="wraps">
    <v-flex
      v-for="(item, index) in imgSrc"
      :key="index"
      shrink
    >
      <v-tooltip top>
        <template v-slot:activator="{ on }">
          <v-card 
            :class="`ma-${ margin }`"
            flat 
            hover 
            v-on="on"
            :min-width="minWidth"
            @mouseover="overDelbtn(item)" 
            @mouseout="outDelbtn(item)" 
            @click.stop="watchMore(index)" 
            v-if="item.fileType === 'image'" 
          >
            <v-img
              :src="item.src"
              :contain="contain"
              :height="height"
            >
              <v-layout row justify-center mt-2>
                <v-badge 
                  color="transparent" 
                  overlap 
                  v-model="item.show"
                >
                  <template v-slot:badge>
                    <v-icon 
                      dark 
                      :size="size" 
                      color="info" 
                      @click.stop="delClick(index, item.isLocal)" 
                    >cancel</v-icon>
                  </template>
                </v-badge>
              </v-layout>
            </v-img>
          </v-card>

          <v-chip
            outline
            close
            v-model="item.show"
            @blur="onBlur(index, item.show, item.isLocal)"
            v-else
          >{{ item.imgName }}</v-chip>
        </template>
        <span>{{ item.imgName }}</span>
      </v-tooltip>
      
    </v-flex>
  </v-layout>
</v-content>
</template>

<script>
export default {
  props: {
    settings: {
      type: Object
    },
    dialogImgSettings: {
      type: Object
    },
    wraps: {
      type: Boolean
    },
    minWidth: {
      type: Number,
      default: 140
    },
    maxWidth: {
      type: Number,
      default: 140
    },
    watchTarget: {
      type: String,
      default: "watch"
    },
    closeTarget: {
      type: String,
      default: "close"
    },
    imgName: {
      type: String
    },
    imgSrc: {
      type: Array
    },
    height: {
      type: Number
    },
    contain: {
      type: Boolean,
      default: false
    },
    show: {
      type: Boolean,
      default: false
    },
    margin: {
      type: Number,
      default: 1
    }
  },
  data: () => ({
    size: 16
  }),
  methods: {
    onBlur(index, isShow, isLocal) {
      if(isShow === false) {
        this.delClick(index, isLocal)
      }
    },
    delClick(index, isLocal) {
      if(!isLocal) {
        let delImgArr = [index, this.imgSrc[index].src]
        this.$emit(this.closeTarget, delImgArr)
        delImgArr = []
      }else {
        this.imgSrc.splice(index, 1)
        this.$emit(this.closeTarget, this.notLocalImgLen(index))
      }
    },
    overDelbtn(item) {
      item.show = true
    },
    outDelbtn(item) {
      item.show = false
    },
    watchMore(index) {
      this.$emit(this.watchTarget, index)
    },
    notLocalImgLen(index) {
      let count = 0
      for(let i = 0; i < this.imgSrc.length; i++) {
        if(!this.imgSrc[i].isLocal) {
          count += 1
        }
      }
      return index - count
    }
  }
}
</script>

<style>

</style>
