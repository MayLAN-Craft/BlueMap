<template>
  <div class="map-button" :class="{selected: map.id === selectedMapId}" @click="switchMap(map.id)" :title="map.id">
    <span class="sky" :style="{color: 'rgb(' + map.skyColor.r * 255 + ',' + map.skyColor.g * 255 + ',' + map.skyColor.b * 255 + ')'}">&bull;</span>
    <span class="name">{{map.name}}</span>
  </div>
</template>

<script>
export default {
  name: "MapButton",
  props: {
    map: Object,
  },
  data() {
    return {
      mapViewer: this.$bluemap.mapViewer.data,
      appState: this.$bluemap.appState,
    }
  },
  computed: {
    selectedMapId() {
      return this.mapViewer.map ? this.mapViewer.map.id : null;
    }
  },
  methods: {
    switchMap(mapId) {
      this.$bluemap.switchMap(mapId);
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .map-button {
  position: relative;
  display: flex;
  align-items: center;

  height: 2em;
  line-height: 2em;

  margin: 0 0 4px 0;
  padding: 0 0.5em $ore-strip 0.5em;

  @include ore-button;

  &.selected {
    @include ore-button-accent;
  }

  .sky {
    flex-shrink: 0;

    // render the sky-color as a little pixel square (bullet text is hidden)
    width: 0.6em;
    height: 0.6em;
    overflow: hidden;
    text-indent: 2em;
    white-space: nowrap;
    background-color: currentColor;
    border: solid $ore-border-width var(--ore-border);

    margin: 0 0.6em 0 0.1em;
  }

  .name {
    white-space: nowrap;
    overflow-x: hidden;
    text-overflow: ellipsis;
  }

  .id {
    font-style: italic;
    color: var(--theme-fg-light);

    margin: 0 0.5em;
  }
}
</style>
