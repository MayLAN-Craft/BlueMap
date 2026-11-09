<template>
  <div class="choice-box">
    <div v-if="title" class="title">{{ title }}</div>
    <div class="choices">
      <div class="choice" v-for="choice of choices" :key="choice.id"
           :class="{selected: selection === choice.id}"
           @click="$emit('choice', choice)"
      >
        {{ choice.name }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChoiceBox",
  props: {
    title: {
      type: String,
      required: false,
      default: ''
    },
    choices: Array,
    selection: String
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

  .choice-box {
    display: flex;

    font-size: 0.8em;
    text-align: center;
    margin: 0 0 0.5em 0;

    border: solid $ore-border-width var(--ore-border);
    overflow: hidden;

    .title, .choice {
      padding: 0.4em 0.5em calc(0.4em + 3px) 0.5em;
    }

    .title {
      background-color: var(--ore-header);
      border-right: solid $ore-border-width var(--ore-border);
    }

    .choices {
      display: flex;
      flex-grow: 1;

      .choice {
        flex-grow: 1;

        cursor: pointer;
        user-select: none;

        background-color: var(--ore-btn);
        box-shadow: inset 0 -3px 0 var(--ore-btn-strip), inset 0 $ore-border-width 0 var(--ore-bevel);

        &:not(:first-child) {
          border-left: solid $ore-border-width var(--ore-border);
        }

        &:hover {
          background-color: var(--ore-btn-hover);
        }

        &.selected {
          background-color: var(--ore-accent);
          color: var(--ore-accent-fg);
          box-shadow: inset 0 -3px 0 var(--ore-accent-strip), inset 0 $ore-border-width 0 rgba(255, 255, 255, 0.25);
        }
      }
    }
  }
</style>
