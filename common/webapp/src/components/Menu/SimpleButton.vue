<template>
<div class="simple-button" :class="{active: active}" @click="$emit('action')">
  <div class="label"><slot /></div>
  <div class="submenu-icon" v-if="submenu">
    <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M5 2h2v1h-2zM5 3h3v1h-3zM6 4h3v1h-3zM7 5h3v1h-3zM8 6h3v1h-3zM9 7h3v1h-3zM9 8h3v1h-3zM8 9h3v1h-3zM7 10h3v1h-3zM6 11h3v1h-3zM5 12h3v1h-3zM5 13h2v1h-2z"/></svg>
  </div>
</div>
</template>

<script>
export default {
  name: "SimpleButton",
  props: {
    submenu: Boolean,
    active: {
      type: Boolean,
      default: false,
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .simple-button {
  display: flex;
  line-height: 2em;

  margin: 0 0 4px 0;
  padding: 0 0.5em $ore-strip 0.5em;

  @include ore-button;

  > .label {
    flex-grow: 1;

    white-space: nowrap;
    overflow-x: hidden;
    text-overflow: ellipsis;
  }

  &.active {
    @include ore-button-accent;

    > .submenu-icon > svg {
      fill: var(--ore-accent-fg);
    }
  }

  &:active {
    padding-top: 2px;
    padding-bottom: $ore-strip - 2px;
  }

  > .submenu-icon {
    width: 2em;
    height: 2em;

    flex-shrink: 0;

    margin-right: -0.5em;

    display: flex;
    align-items: center;
    justify-content: center;

    > svg {
      display: block;
      width: 32px;
      height: 32px;
      fill: var(--theme-fg);
    }
  }
}
</style>
