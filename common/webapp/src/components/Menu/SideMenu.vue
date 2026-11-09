<template>
  <Transition name="side-menu" @enter="buttonEnterAnimation(); $emit('enter', $event)">
    <div class="side-menu" v-if="open">
      <MenuButton :close="open && rendered" :back="back" @action="$emit('back', $event)" />
      <MenuButton class="full-close" v-if="open && back" :close="true" @action="$emit('close', $event)" />
      <div class="title">{{ title }}</div>
      <div class="content">
        <slot />
      </div>
    </div>
  </Transition>
</template>

<script>
import MenuButton from "../ControlBar/MenuButton.vue";

export default {
  name: "SideMenu",
  components: {MenuButton},
  props: {
    title: {
      type: String,
      default: "Menu"
    },
    open: {
      type: Boolean,
      default: true
    },
    back: Boolean
  },
  data() {
    return {
      rendered: false
    }
  },
  methods: {
    async buttonEnterAnimation() {
      this.rendered = false;
      await this.$nextTick();
      await this.$nextTick();
      this.rendered = true;
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu {
  position: fixed;
  top: 0;
  left: 0;

  overflow: hidden;

  pointer-events: auto;

  box-sizing: border-box;
  width: 100%;
  max-width: 22em;
  height: 100%;

  @include ore-drop-shadow;

  background-color: var(--theme-bg);
  color: var(--theme-fg);
  border-right: solid $ore-border-width var(--ore-border);

  &-enter-active, &-leave-active {
    transition: opacity 0.2s, transform 0.2s;
  }

  &-enter-from, &-leave-to {
    opacity: 0;
    transform: translateX(-1em);
    pointer-events: none;

    * {
      pointer-events: none !important;
    }
  }

  > .menu-button {
    position: absolute;
    top: 0;
    left: 0;

    margin: 0.5em;

    @media (max-width: $mobile-break) {
      margin: 0;
    }

    &.full-close {
      right: 0;
      left: unset;
    }
  }

  > .title {
    line-height: 2em;
    text-align: center;

    background-color: var(--ore-header);
    border-bottom: solid $ore-border-width var(--ore-border);
    box-shadow: inset 0 $ore-border-width 0 var(--ore-bevel);

    padding: 0.5em 3em;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;

    @media (max-width: $mobile-break) {
      padding: 0 2.5em;
    }
  }

  > .content {
    position: relative;
    box-sizing: border-box;

    overflow-y: auto;
    overflow-x: hidden;

    padding: 0.5em;

    height: calc(100% - 3em - #{$ore-border-width});
    @media (max-width: $mobile-break) {
      height: calc(100% - 2em - #{$ore-border-width});
    }

    font-size: 0.9em;

    hr {
      border: none;
      border-bottom: solid $ore-border-width var(--ore-border);
      margin: 0.75em 0;
    }
  }
}
</style>
