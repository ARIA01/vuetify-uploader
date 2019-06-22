<template>
  <div class="text-xs-center">
    <v-dialog
      v-model="showDialog"
      :width="width"
      :min-width="width"
      min-height="750"
      :scrollable="false"
      persistent
    >
      <v-card>
        <v-layout row justify-center>
          <v-flex xs12>
            <v-card flat style="overflow-x: hidden; overflow-y: auto;" min-height="750">
              <slot name="images"></slot>
            </v-card>
          </v-flex>
        </v-layout>

        <v-card-actions class="headline grey lighten-3" :style="`display: ${ display }`">
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click.native="closeDialog"
          >关闭</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: Number,
      default: 800
    },
    showDialog: {
      type: Boolean,
      default: false
    },
    display: {
      type: String
    }
  },
  data: () => ({
  }),
  methods: {
    closeDialog() {
      this.$emit("close")
    }
  },
  computed: {
  }
}
</script>

<style>
</style>
