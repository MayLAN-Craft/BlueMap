<template>
  <SvgButton class="compass" @action="action">
    <svg viewBox="0 0 16 16" shape-rendering="crispEdges" :style="style"><path class="north" d="M7 1h2v1h-2zM7 2h2v1h-2zM6 3h4v1h-4zM6 4h4v1h-4zM5 5h6v1h-6zM5 6h6v1h-6zM4 7h8v1h-8z"/><path class="south" d="M4 9h8v1h-8zM5 10h6v1h-6zM5 11h6v1h-6zM6 12h4v1h-4zM6 13h4v1h-4zM7 14h2v1h-2zM7 15h2v1h-2z"/></svg>
  </SvgButton>
</template>

<script>
  import {animate, EasingFunctions} from "../../js/util/Utils";
  import SvgButton from "./SvgButton.vue";

  let animation;

  export default {
    name: "Compass",
    components: {SvgButton},
    data() {
      return {
        controls: this.$bluemap.mapViewer.controlsManager.data
      }
    },
    computed: {
      style() {
        return {transform: "translate(-50%, -50%) rotate(" + (-this.controls.rotation) + "rad)"}
      }
    },
    methods: {
      action(evt) {
        evt.preventDefault();

        if (animation) animation.cancel();

        let startRotation = this.controls.rotation;
        animation = animate(t => {
          this.controls.rotation = startRotation * (1-EasingFunctions.easeOutQuad(t));
        }, 300);
      }
    }
  }
</script>

<style lang="scss">
  .compass {
    svg {
      height: 1.6em;

      .north {
        fill: #e2483b;
      }

      .south {
        fill: var(--theme-fg);
      }
    }
  }
</style>