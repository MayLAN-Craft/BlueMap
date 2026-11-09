<template>
  <div id="ff-mobile-controls" :class="{disabled: !enabled}">
    <div class="move-fields">
      <div class="button up-button" @touchstart.passive="forward = 1; forwardPointer = $event.changedTouches[0].identifier; $event.preventDefault();">
        <svg viewBox="0 0 16 8" shape-rendering="crispEdges"><path d="M7 0h2v1h-2zM6 1h4v1h-4zM5 2h6v1h-6zM4 3h8v1h-8zM3 4h10v1h-10zM2 5h12v1h-12zM1 6h14v1h-14zM0 7h16v1h-16z"/></svg>
      </div>
      <div class="button down-button" @touchstart.passive="forward = -1; forwardPointer = $event.changedTouches[0].identifier; $event.preventDefault();">
        <svg viewBox="0 0 16 8" shape-rendering="crispEdges" class="down"><path d="M7 0h2v1h-2zM6 1h4v1h-4zM5 2h6v1h-6zM4 3h8v1h-8zM3 4h10v1h-10zM2 5h12v1h-12zM1 6h14v1h-14zM0 7h16v1h-16z"/></svg>
      </div>
    </div>
    <div class="height-fields">
      <div class="button up-button" @touchstart.passive="up = 1; upPointer = $event.changedTouches[0].identifier; $event.preventDefault();">
        <svg viewBox="0 0 16 8" shape-rendering="crispEdges"><path d="M7 0h2v1h-2zM6 1h4v1h-4zM5 2h6v1h-6zM4 3h8v1h-8zM3 4h10v1h-10zM2 5h12v1h-12zM1 6h14v1h-14zM0 7h16v1h-16z"/></svg>
      </div>
      <div class="button down-button" @touchstart.passive="up = -1; upPointer = $event.changedTouches[0].identifier; $event.preventDefault();">
        <svg viewBox="0 0 16 8" shape-rendering="crispEdges" class="down"><path d="M7 0h2v1h-2zM6 1h4v1h-4zM5 2h6v1h-6zM4 3h8v1h-8zM3 4h10v1h-10zM2 5h12v1h-12zM1 6h14v1h-14zM0 7h16v1h-16z"/></svg>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FreeFlightMobileControls",
  data() {
    return {
      enabled: false,
      forward: 0,
      forwardPointer: -1,
      up: 0,
      upPointer: -1,
    }
  },
  methods: {
    onTouchStop(evt) {
      console.log("Stop: ", evt);

      if (evt.changedTouches[0].identifier === this.forwardPointer) this.forward = 0;
      if (evt.changedTouches[0].identifier === this.upPointer) this.up = 0;
    },
    onFrame(evt) {
      let cm = this.$bluemap.mapViewer.controlsManager;
      cm.position.x += this.forward * Math.sin(cm.rotation) * evt.detail.delta * 0.02;
      cm.position.z += this.forward * -Math.cos(cm.rotation) * evt.detail.delta * 0.02;
      cm.position.y += this.up * evt.detail.delta * 0.01;
    },
    enable() {
      this.enabled = true;
    },
  },
  mounted() {
    window.addEventListener("touchstart", this.enable, { passive: true });

    window.addEventListener("touchend", this.onTouchStop);
    window.addEventListener("touchcancel", this.onTouchStop);
    this.$bluemap.events.addEventListener("bluemapRenderFrame", this.onFrame);
  },
  beforeUnmount() {
    window.removeEventListener("touchstart", this.enable);

    window.removeEventListener("touchend", this.onTouchStop);
    window.removeEventListener("touchcancel", this.onTouchStop);
    this.$bluemap.events.removeEventListener("bluemapRenderFrame", this.onFrame);
  }
}
</script>

<style lang="scss">

#ff-mobile-controls {
  font-size: 15vw;

  &.disabled {
    display: none;
  }

  @media (orientation: portrait) {
    font-size: 15vh;
  }

  .button {
    width: 1em;
    margin: 0.1em;
    opacity: 0.5;

    pointer-events: auto;

    svg {
      fill: var(--ore-btn);
      filter: drop-shadow(0 2px 0 var(--ore-border)) drop-shadow(0 -2px 0 var(--ore-border));
      overflow: visible;

      &:active {
        fill: var(--ore-accent);
        opacity: 0.8;
      }

      &.down {
        transform: scaleY(-1);
      }
    }
  }

  .move-fields {
    position: fixed;

    bottom: 0.2em;
    left: 0.2em;

  }

  .height-fields {
    position: fixed;

    bottom: 0.2em;
    right: 0.2em;
  }

}

</style>