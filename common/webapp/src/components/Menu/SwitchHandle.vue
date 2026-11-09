<template>
  <div class="switch" :class="{on: on}"></div>
</template>

<script>
export default {
  name: "SwitchHandle",
  props: {
    on: Boolean
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

// Ore UI toggle: rectangular sunken track with a raised square knob
.side-menu .switch {
  $w: 2.4em;
  $h: 1.1em;

  position: relative;
  flex-shrink: 0;

  width: $w;
  height: $h;

  background-color: var(--ore-field);
  border: solid $ore-border-width var(--ore-border);

  transition: background-color 0.15s;

  // "on" tick mark on the left side of the track
  &::before {
    content: "";
    position: absolute;
    top: 25%;
    left: 0.45em;
    width: $ore-border-width;
    height: 50%;
    background-color: var(--ore-accent-fg);
    opacity: 0;
    transition: opacity 0.15s;
  }

  &::after {
    content: "";
    position: absolute;
    top: -$ore-border-width;
    left: -$ore-border-width;

    width: $h;
    height: $h;

    background-color: var(--ore-btn);
    border: solid $ore-border-width var(--ore-border);
    box-shadow: inset 0 -3px 0 var(--ore-btn-strip), inset 0 $ore-border-width 0 var(--ore-bevel);

    transition: left 0.15s;
  }

  &.on {
    background-color: var(--ore-accent);

    &::before {
      opacity: 1;
    }

    &::after {
      left: calc(#{$w - $h} - #{$ore-border-width});
    }
  }
}
</style>
