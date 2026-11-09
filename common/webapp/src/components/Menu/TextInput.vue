<template>
  <input class="text-input" type="text" :value="value" @input="$emit('input', $event)" @keydown="$event.stopPropagation()">
</template>

<script>
export default {
  name: "TextInput",
  props: {
    value: String
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .text-input {
  @include ore-field;

  width: calc(100% - 1em - #{$ore-border-width * 2});
  padding: 0.5em;
  margin: 0 0 0.5em 0;

  &::placeholder {
    color: var(--theme-fg-light);
    opacity: 1;
  }

  &:focus {
    border-color: var(--ore-accent);
  }
}
</style>
