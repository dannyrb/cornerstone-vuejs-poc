<template>
  <div class="page-container">
    <h1>{{msg}}</h1>

    <cornerstone-canvas></cornerstone-canvas>

  </div>
</template>

<script>
import CornerstoneCanvas from './../components/CornerstoneCanvas'

export default {
  name: 'DefaultRoute',
  components: {
    CornerstoneCanvas
  },
  data () {
    return {
      msg: 'Hello Cornerstone (:'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
