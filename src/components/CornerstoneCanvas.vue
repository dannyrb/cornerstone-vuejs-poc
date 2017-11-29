<template>
  <!-- WRAPPER -->
  <div
    class="image-canvas-wrapper"
    oncontextmenu="return false"
    unselectable='on'
    onselectstart='return false;'
    onmousedown='return false;'
  >

    <!-- DICOM CANVAS -->
    <div
      ref="canvas"
      class="image-canvas"
      oncontextmenu="return false"
    ></div>

  </div>

</template>

<script>
import $ from 'jquery'
import Hammer from 'hammerjs'
import * as cornerstone from 'cornerstone-core'
import * as cornerstoneTools from 'cornerstone-tools'
import * as cornerstoneWebImageLoader from 'cornerstone-web-image-loader'

cornerstone.external.$ = $
cornerstoneTools.external.cornerstone = cornerstone
cornerstoneTools.external.$ = $
cornerstoneTools.external.Hammer = Hammer
cornerstoneWebImageLoader.external.$ = $
cornerstoneWebImageLoader.external.cornerstone = cornerstone

cornerstone.registerImageLoader('http', cornerstoneWebImageLoader.loadImage)
cornerstone.registerImageLoader('https', cornerstoneWebImageLoader.loadImage)

export default {
  name: 'CornerstoneCanvas',
  data () {
    return {
      browserWindowWidth: 0,
      browserWindowHeight: 0,
      selectedImageIdIndex: 0,
      exampleStudyImageIds: [
        '/static/simple-study/1.2.276.0.74.3.1167540280.200511.112514.1.1.4.jpg'
      ]
    }
  },
  mounted () {
    // this.listenForCornerstoneImageRendered()
    // this.listenForCornerstoneImageLoaded()
    this.listenForWindowResize()

    // Enable Canvas
    let canvas = this.$refs.canvas
    cornerstone.enable(canvas)

    // Load first series
    // let initTools = true
    // this.loadSeries(initTools)

    const imageUrl = 'http://localhost:8080' + this.exampleStudyImageIds[0]
    cornerstone.loadImage(imageUrl).then(function (image) {
      cornerstone.displayImage(canvas, image)
    })
  },
  beforeDestroy () {
    // Remove jQuery event listeners
    let canvas = this.$refs.canvas
    $(canvas).off()
  },
  methods: {
    listenForWindowResize: function () {
      this.$nextTick(function () {
        window.addEventListener('resize', this.debounce(this.onWindowResize, 100))
      })
    },
    onWindowResize: function () {
      cornerstone.resize(this.$refs.canvas, true)
    },
    // Util
    debounce: function (func, wait, immediate) {
      var timeout
      return function () {
        var context = this
        var args = arguments
        var later = function () {
          timeout = null
          if (!immediate) func.apply(context, args)
        }
        var callNow = immediate && !timeout
        clearTimeout(timeout)
        timeout = setTimeout(later, wait)
        if (callNow) func.apply(context, args)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-canvas-wrapper {
  width: 100%;
  height: 525px;
}

.image-canvas {
  width: 100%;
  height: 100%;
}
</style>
