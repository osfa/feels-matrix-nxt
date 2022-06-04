<template>
  <client-only placeholder="...Loading">
    <div style="width: 100%; height: 100%" id="viewer-image" ref="image"></div>
  </client-only>
</template>
<script src="/openseadragon/openseadragon.min.js"></script>
<script>
if (process.client) {
  require('openseadragon')
}
import OpenSeadragon from 'use-open-seadragon/lib/types/open-seadragon'
// const ts = {
//   Image: {
//     xmlns: 'http://schemas.microsoft.com/deepzoom/2008',
//     Url: '/dzi/default-dzi-woj1.1_files/',
//     Format: 'jpeg',
//     Overlap: '1',
//     TileSize: '254',
//     ServerFormat: 'Default',
//     Size: {
//       Height: '58880',
//       Width: '58880',
//     },
//   },
// }

// const viewer = {
//   ...OpenSeadragon,
//   id: 'viewer-image',
//   maxImageCacheCount: 5000,
//   tileSources: ts,
//   // imageSmoothingEnabled???
//   showNavigator: false,
//   showRotationControl: false,
//   animationTime: 1, // 5,
//   blendTime: 0.1, // 0.5,
//   showNavigationControl: false,

//   visibilityRatio: 1, // ?
//   // constrainDuringPan: true, // prevents bounceback anim
//   autoResize: true, // viewport resize adapt
//   zoomPerScroll: 1.2,
//   defaultZoomLevel: 6, // 2 if mobile, calc based on width?
//   minZoomLevel: 6, // 2 if mobile calc based on with?
//   // maxZoomPixelRatio: 2, // default 1.1 The maximum ratio to allow a zoom-in to affect the highest level pixel ratio.
//   // This can be set to Infinity to allow 'infinite' zooming into the image
//   // maxZoomLevel: 24,
//   wrapHorizontal: true,
//   wrapVertical: true,
// }
export default {
  title: 'OpenSeaDragon',
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
  mounted() {
    this.initViewer()
  },
  methods: {
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
