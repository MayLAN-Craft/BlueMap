<template>
  <div class="svg-button" :class="{active: active}" @click="$emit('action', $event)">
    <slot />
  </div>
</template>

<script>
  export default {
    name: "SvgButton",
    props: {
      active: Boolean,
    }
  }
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.svg-button {
  position: relative;
  pointer-events: auto;
  overflow: hidden;

  box-sizing: border-box;
  min-width: 2em;
  min-height: 2em;

  @include ore-button;

  &.active {
    @include ore-button-accent;

    svg {
      fill: var(--ore-accent-fg);
    }
  }

  svg {
    position: absolute;
    top: calc(50% - #{$ore-strip * 0.5});
    left: 50%;
    transform: translate(-50%, -50%);

    height: 1.6em;

    fill: var(--theme-fg);
    shape-rendering: crispEdges;
  }

  // pressed: the icon sinks down with the button face
  &:active svg {
    top: 50%;
  }
}
</style>
