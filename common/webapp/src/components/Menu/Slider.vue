<template>
<div class="slider" :style="{'--value': percent}">
  <div class="label"><slot />: <span class="value">{{formatter(value)}}</span></div>
  <label>
    <input type="range" :min="min" :max="max" :step="step" :value="value" @input="$emit('update', parseFloat($event.target.value))" @change="$emit('lazy', parseFloat($event.target.value))">
  </label>
</div>
</template>

<script>

function countDecimals(value) {
  if(Math.floor(value) === value) return 0;
  return value.toString().split(".")[1].length || 0;
}

export default {
  name: "Slider",
  props: {
    value: Number,
    min: Number,
    max: Number,
    step: Number,
    formatter: {
      type: Function,
      default: function(value) {
        return parseFloat(value).toFixed(countDecimals(this.step));
      }
    }
  },
  computed: {
    percent() {
      if (this.max === this.min) return "0%";
      let p = (this.value - this.min) / (this.max - this.min);
      return (Math.min(Math.max(p, 0), 1) * 100) + "%";
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

@mixin slider-thumb {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  box-sizing: border-box;

  width: 1.1em;
  height: 1.4em;

  border-radius: 0;
  border: solid $ore-border-width var(--ore-border);
  background-color: var(--ore-btn);
  box-shadow: inset 0 -3px 0 var(--ore-btn-strip), inset 0 $ore-border-width 0 var(--ore-bevel);
}

.side-menu .slider {
  line-height: 2em;
  margin: 0 0 4px 0;
  padding: 0 0.5em 0.5em 0.5em;

  border: solid $ore-border-width transparent;

  &:hover {
    background-color: var(--theme-bg-hover);
    border-color: var(--ore-border);
  }

  > .label {
    > .value {
      float: right;
      color: var(--theme-fg-light);
    }
  }

  > label {
    display: block;
    padding: 0 0.6em;

    > input {
      appearance: none;
      -moz-appearance: none;
      -webkit-appearance: none;
      outline: none;
      cursor: pointer;

      display: block;
      box-sizing: border-box;
      width: 100%;
      height: 0.6em;
      margin: 0.4em 0;

      border: solid $ore-border-width var(--ore-border);
      background: linear-gradient(to right, var(--ore-accent) var(--value, 0%), var(--ore-field) var(--value, 0%));

      &::-webkit-slider-thumb {
        @include slider-thumb;
      }

      &::-moz-range-thumb {
        @include slider-thumb;
      }

      &:hover::-webkit-slider-thumb {
        background-color: var(--ore-btn-hover);
      }

      &:hover::-moz-range-thumb {
        background-color: var(--ore-btn-hover);
      }
    }
  }
}
</style>
