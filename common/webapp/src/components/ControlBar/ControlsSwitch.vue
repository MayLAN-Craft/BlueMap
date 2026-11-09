<template>
  <div class="controls-switch">
    <SvgButton v-if="mapViewer.map.perspectiveView" :active="isPerspectiveView" @action="setPerspectiveView" :title="$t('controls.perspective.tooltip')">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M7 3h2v1h-2zM5 4h6v1h-6zM3 5h10v1h-10zM1 6h14v1h-14zM1 7h14v1h-14zM1 8h14v1h-14zM3 9h10v1h-10zM5 10h6v1h-6zM7 11h2v1h-2z"/></svg>
    </SvgButton>
    <SvgButton v-if="mapViewer.map.flatView" :active="isFlatView" @action="setFlatView" :title="$t('controls.flatView.tooltip')">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M3 3h10v1h-10zM3 4h10v1h-10zM3 5h10v1h-10zM3 6h10v1h-10zM3 7h10v1h-10zM3 8h10v1h-10zM3 9h10v1h-10zM3 10h10v1h-10zM3 11h10v1h-10zM3 12h10v1h-10z"/></svg>
    </SvgButton>
    <SvgButton v-if="mapViewer.map.freeFlightView" :active="isFreeFlight" @action="setFreeFlight" :title="$t('controls.freeFlight.tooltip')">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M6 1h4v1h-4zM6 2h4v1h-4zM6 3h4v1h-4zM6 4h4v1h-4zM3 6h10v1h-10zM2 7h12v1h-12zM1 8h2v1h-2zM4 8h8v1h-8zM13 8h2v1h-2zM1 9h1v1h-1zM4 9h8v1h-8zM14 9h1v1h-1zM4 10h3v1h-3zM9 10h3v1h-3zM4 11h3v1h-3zM9 11h3v1h-3zM4 12h3v1h-3zM9 12h3v1h-3zM4 13h3v1h-3zM9 13h3v1h-3z"/></svg>
    </SvgButton>
  </div>
</template>

<script>
  import SvgButton from "./SvgButton.vue";

  export default {
    name: "ControlsSwitch",
    components: {SvgButton},
    data() {
      return {
        controls: this.$bluemap.appState.controls,
        mapViewer: this.$bluemap.mapViewer.data
      }
    },
    computed: {
      isPerspectiveView() {
        return this.controls.state === "perspective";
      },
      isFlatView() {
        return this.controls.state === "flat";
      },
      isFreeFlight() {
        return this.controls.state === "free";
      }
    },
    methods: {
      setPerspectiveView() {
        this.$bluemap.setPerspectiveView(500, this.isFreeFlight ? 100 : 0);
      },
      setFlatView() {
        this.$bluemap.setFlatView(500, this.isFreeFlight ? 100 : 0);
      },
      setFreeFlight() {
        this.$bluemap.setFreeFlight(500);
      }
    }
  }
</script>

<style lang="scss">
  .controls-switch {
    display: flex;

    // segmented group: neighbouring buttons share one outline
    > :not(:first-child) {
      margin-left: -2px;
    }
  }
</style>