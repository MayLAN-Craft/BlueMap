<template>
  <div id="zoom-buttons">
    <SvgButton @action="zoom(-3)">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M7 2h2v1h-2zM7 3h2v1h-2zM7 4h2v1h-2zM7 5h2v1h-2zM7 6h2v1h-2zM2 7h12v1h-12zM2 8h12v1h-12zM7 9h2v1h-2zM7 10h2v1h-2zM7 11h2v1h-2zM7 12h2v1h-2zM7 13h2v1h-2z"/></svg>
    </SvgButton>
    <SvgButton @action="zoom(3)">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M2 7h12v1h-12zM2 8h12v1h-12z"/></svg>
    </SvgButton>
  </div>
</template>

<script>
import SvgButton from "../ControlBar/SvgButton.vue";

export default {
  name: "ZoomButtons",
  components: {
    SvgButton
  },
  methods: {
    zoom(delta) {
      let mouseZoom = this.$bluemap.mapViewer.controlsManager.controls?.mouseZoom;
      if (mouseZoom) {
        mouseZoom.deltaZoom += delta;
      }
    }
  }
}
</script>

<style lang="scss">
  #zoom-buttons {
    position: fixed;
    bottom: 0;
    right: 0;

    display: flex;
    flex-direction: column;

    gap: 4px;
    filter: drop-shadow(3px 3px 0 var(--ore-shadow));
    width: 2em;

    margin: 0.5em;
  }
</style>