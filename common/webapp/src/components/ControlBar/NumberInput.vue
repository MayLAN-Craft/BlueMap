<template>
  <div class="number-input">
    <label>
      <span class="label">{{label}}:</span>
      <input type="number"
             v-bind:value="format(value)"
             v-on:input="$emit('input', $event)"
             v-on:keydown="$event.stopPropagation()"
      >
    </label>
  </div>
</template>

<script>
export default {
  name: "NumberInput",
  props: {
    label: String,
    value: Number
  },
  computed: {
    format() {
      return (value) => {
        return Math.floor(value);
      }
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

  .number-input {
    pointer-events: auto;
    box-sizing: border-box;

    @include ore-field;

    min-height: 2em;
    line-height: calc(2em - #{$ore-border-width * 2});

    label {
      display: flex;
      height: 100%;
    }

    .label {
      flex-shrink: 0;
      padding: 0 0.25em 0 0.5em;

      color: var(--ore-accent-hover);
    }

    input {
      flex-grow: 1;
      min-width: 0;
      height: 100%;
      line-height: inherit;
      width: 100%;

      background-color: transparent;
      color: inherit;

      // remove number spinner firefox
      -moz-appearance: textfield;

      // remove number spinner webkit
      &::-webkit-inner-spin-button,
      &::-webkit-outer-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
    }

    &:focus-within {
      border-color: var(--ore-accent);
    }
  }
</style>
