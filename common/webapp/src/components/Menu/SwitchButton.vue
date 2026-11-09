<template>
<div class="switch-button" @click="$emit('action')">
  <div class="label"><slot /></div>
  <SwitchHandle :on="on" />
</div>
</template>

<script>
import SwitchHandle from "./SwitchHandle.vue";
export default {
  name: "SwitchButton",
  components: {SwitchHandle},
  props: {
    on: Boolean
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .switch-button {
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
  gap: 0.5em;
  line-height: 2em;

  margin: 0 0 4px 0;
  padding: 0.1em 0.5em;

  border: solid $ore-border-width transparent;

  > .label {
    flex-grow: 1;

    white-space: nowrap;
    overflow-x: hidden;
    text-overflow: ellipsis;
  }

  &:hover {
    background-color: var(--theme-bg-hover);
    border-color: var(--ore-border);
  }
}
</style>
