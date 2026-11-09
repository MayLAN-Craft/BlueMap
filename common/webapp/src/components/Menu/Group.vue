<template>
  <div class="group">
    <span class="title">{{ title }}</span>
    <div class="content">
      <slot/>
    </div>
  </div>
</template>

<script>
export default {
  name: "Group",
  props: {
    title: String
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .group {
  position: relative;
  margin: 1em 0;

  background-color: var(--ore-group);
  border: solid $ore-border-width var(--ore-border);

  > .title {
    display: block;
    padding: 0.4em 0.5em;

    color: var(--theme-fg-light);
    background-color: var(--ore-header);
    border-bottom: solid $ore-border-width var(--ore-border);

    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  &:first-child {
    margin-top: 0;
  }

  > .content {
    max-height: 15em;
    overflow-y: auto;
    padding: 0.5em 0.5em calc(0.5em - 4px) 0.5em;
  }
}
</style>
