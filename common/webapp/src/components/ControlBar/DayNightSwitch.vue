<template>
  <SvgButton class="day-night-switch" :active="!isDay" @action="action">
    <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M5 2h5v1h-5zM3 3h5v1h-5zM2 4h5v1h-5zM2 5h4v1h-4zM1 6h5v1h-5zM1 7h5v1h-5zM1 8h5v1h-5zM1 9h5v1h-5zM2 10h4v1h-4zM2 11h5v1h-5zM3 12h5v1h-5zM5 13h5v1h-5z"/><path class="stars" d="M13 2h1v1h-1zM12 6h2v1h-2zM11 7h4v1h-4zM11 8h4v1h-4zM12 9h2v1h-2zM13 13h1v1h-1z"/></svg>
  </SvgButton>
</template>

<script>
import {animate, EasingFunctions} from "../../js/util/Utils";
import SvgButton from "./SvgButton.vue";

let animation;

export default {
    name: "DayNightSwitch",
  components: {SvgButton},
  data() {
      return {
        mapViewer: this.$bluemap.mapViewer.data
      }
    },
    computed: {
      isDay() {
        return this.mapViewer.uniforms.sunlightStrength.value > 0.6;
      }
    },
    methods: {
      action(evt) {

        evt.preventDefault();

        if (animation) animation.cancel();

        let startValue = this.mapViewer.uniforms.sunlightStrength.value;
        let targetValue = this.isDay ? 0.25 : 1;
        animation = animate(t => {
          let u = EasingFunctions.easeOutQuad(t);
          this.mapViewer.uniforms.sunlightStrength.value = startValue * (1-u) + targetValue * u;
          this.$bluemap.mapViewer.redraw();
        }, 300);
      }
    }
  }
</script>

<style lang="scss">
  .day-night-switch {
    svg {
      fill: var(--theme-moon-day);
      .stars {
        fill: var(--theme-stars-day);
      }
    }

    &.active {
      svg {
        fill: var(--theme-moon-night);
        .stars {
          fill: var(--theme-stars-night);
        }
      }
    }
  }
</style>