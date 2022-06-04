<template>
  <div id="viewer-image" ref="image"></div>
</template>
<script>
import OpenSeadragon from 'openseadragon'
window.OpenSeadragon = OpenSeadragon

export default {
  name: 'FeelsViewer',
  data() {
    return {
      viewer: null,
      timer: null,
      interval: 5000,
      startZoom: 6,
      minZoom: 6,
      maxZoom: 24,
      moveStep: 0.1,
    }
  },
  // created() {

  // },
  destroyed() {
    clearTimeout(this.timer)
  },
  mounted() {
    this.initViewer()
  },
  methods: {
    randomInt(min, max) {
      min = Math.ceil(min)
      max = Math.floor(max)
      return Math.floor(Math.random() * (max - min)) + min
    },
    randomFloat(min, max) {
      const r = Math.random() * (max - min) + min
      console.log(r)
      return r
    },
    // randomCoord() {
    //   return new OpenSeadragon.Point(Math.random(), Math.random());
    // },
    randomCoord(point, d) {
      console.log(point)
      const coord = new OpenSeadragon.Point(
        point.x + this.randomFloat(0, d),
        point.y + this.randomFloat(0, d)
      )
      return coord
    },
    move() {
      console.log('move')
      // var randomZoom = this.randomInt(2, 20);
      const center = this.viewer.viewport.getCenter(true)
      const randomPos = this.randomCoord(center, this.moveStep)

      // this.viewer.viewport.zoomTo(randomZoom, randomPos);
      this.viewer.viewport.panTo(randomPos)
    },
    initViewer() {
      console.log('init Viewer')

      const ts = {
        Image: {
          xmlns: 'http://schemas.microsoft.com/deepzoom/2008',
          Url: '/dzi/default-dzi-woj1.1_files/',
          Format: 'jpeg',
          Overlap: '1',
          TileSize: '254',
          ServerFormat: 'Default',
          Size: {
            Height: '58880',
            Width: '58880',
          },
        },
      }

      this.viewer = OpenSeadragon({
        id: 'viewer-image',
        maxImageCacheCount: 5000,
        tileSources: ts,
        // imageSmoothingEnabled???
        showNavigator: false,
        showRotationControl: false,
        animationTime: 1, // 5,
        blendTime: 0.1, // 0.5,
        showNavigationControl: false,

        visibilityRatio: 1, // ?
        // constrainDuringPan: true, // prevents bounceback anim
        autoResize: true, // viewport resize adapt
        zoomPerScroll: 1.2,
        defaultZoomLevel: this.startZoom, // 2 if mobile, calc based on width?
        minZoomLevel: this.minZoom, // 2 if mobile calc based on with?
        // maxZoomPixelRatio: 2, // default 1.1 The maximum ratio to allow a zoom-in to affect the highest level pixel ratio.
        // This can be set to Infinity to allow 'infinite' zooming into the image
        // maxZoomLevel: 24,
        wrapHorizontal: true,
        wrapVertical: true,
      })

      this.viewer.addHandler('open', () => {
        this.timer = setInterval(() => {
          this.move()
        }, this.interval)
        this.viewer.addHandler('zoom', () => {
          console.log('zoom', this.viewer.viewport.getZoom())
          // howl trigger...
        })
      })
      // this.viewer.initializeAnnotations()
    },
  },
}
</script>
<style>
#viewer-image {
  height: 100%;
  width: 100%;
}
</style>
