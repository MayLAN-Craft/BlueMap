<template>
  <SvgButton class="menu-button" :class="{close: close, back: back}">
    <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path class="icon-menu" d="M2 3h12v1h-12zM2 4h12v1h-12zM2 7h12v1h-12zM2 8h12v1h-12zM2 11h12v1h-12zM2 12h12v1h-12z"/><path class="icon-close" d="M2 2h2v1h-2zM12 2h2v1h-2zM2 3h3v1h-3zM11 3h3v1h-3zM3 4h3v1h-3zM10 4h3v1h-3zM4 5h3v1h-3zM9 5h3v1h-3zM5 6h6v1h-6zM6 7h4v1h-4zM6 8h4v1h-4zM5 9h6v1h-6zM4 10h3v1h-3zM9 10h3v1h-3zM3 11h3v1h-3zM10 11h3v1h-3zM2 12h3v1h-3zM11 12h3v1h-3zM2 13h2v1h-2zM12 13h2v1h-2z"/><path class="icon-back" d="M9 2h2v1h-2zM8 3h3v1h-3zM7 4h3v1h-3zM6 5h3v1h-3zM5 6h3v1h-3zM4 7h3v1h-3zM4 8h3v1h-3zM5 9h3v1h-3zM6 10h3v1h-3zM7 11h3v1h-3zM8 12h3v1h-3zM9 13h2v1h-2z"/></svg>
  </SvgButton>
</template>

<script>
import SvgButton from "./SvgButton.vue";
export default {
  name: "MenuButton",
  components: {SvgButton},
  props: {
    close: Boolean,
    back: Boolean,
  }
}
</script>

<style lang="scss">
  // pixel icons: hamburger / close / back are swapped instead of animated
  .menu-button {
    svg {
      .icon-close, .icon-back {
        display: none;
      }
    }

    &.close {
      svg {
        .icon-menu {
          display: none;
        }
      }

      &:not(.back) svg .icon-close {
        display: inline;
      }

      &.back svg .icon-back {
        display: inline;
      }
    }
  }
</style>
