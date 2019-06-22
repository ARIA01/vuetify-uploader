<template>
  <v-app>
    <c-main></c-main>
  </v-app>
</template>

<script>
import Main from './views/Main'

export default {
  name: 'App',
  components: {
    "c-main": Main
  },
  data () {
    return {
    }
  }
}
</script>
