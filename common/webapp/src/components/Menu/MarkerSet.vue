<template>
  <div class="marker-set" :title="markerSet.id">
    <div class="info" @click="toggle">
      <div class="marker-set-switch">
        <div class="label">{{ label }}</div>
        <SwitchHandle :on="markerSet.visible" v-if="markerSet.toggleable"/>
      </div>
      <div class="stats">
        <div v-if="filteredMarkerCount > 0">
          {{ filteredMarkerCount }}
          {{ $t('markers.marker', filteredMarkerCount) }}
        </div>
        <div v-if="filteredMarkerSetCount > 0">
          {{ filteredMarkerSetCount }}
          {{ $t('markers.markerSet', filteredMarkerSetCount) }}
        </div>
      </div>
    </div>
    <div class="open-menu-button"
         :class="{active: active}"
         @click="more($event)">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M5 2h2v1h-2zM5 3h3v1h-3zM6 4h3v1h-3zM7 5h3v1h-3zM8 6h3v1h-3zM9 7h3v1h-3zM9 8h3v1h-3zM8 9h3v1h-3zM7 10h3v1h-3zM6 11h3v1h-3zM5 12h3v1h-3zM5 13h2v1h-2z"/></svg>
    </div>
  </div>
</template>

<script>
import SwitchHandle from "./SwitchHandle.vue";

export default {
  name: "MarkerSet",
  components: {SwitchHandle},
  props: {
    markerSet: Object,
  },
  computed: {
    filteredMarkerSetCount() {
      let count = 0;
      for (let markerSet of this.markerSet.markerSets) {
        if (markerSet.listed) count++;
      }
      return count;
    },
    filteredMarkerCount() {
      let count = 0;
      for (let marker of this.markerSet.markers) {
        if (marker.listed) count++;
      }
      return count;
    },
    label() {
      if (this.markerSet.id === "bm-players") return this.$t("players.title");
      return this.markerSet.label;
    },
    active() {
      for (let marker of this.markerSet.markers) {
        if (marker.listed) return true;
      }
      for (let markerSet of this.markerSet.markerSets) {
        if (markerSet.listed) return true;
      }
      return false;
    }
  },
  methods: {
    toggle() {
      if (this.markerSet.toggleable) {
        // eslint-disable-next-line vue/no-mutating-props
        this.markerSet.visible = !this.markerSet.visible
        this.markerSet.saveState();
      }
    },
    more(event) {
      if (this.active) {
        this.$emit('more', event);
      }
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .marker-set {
  display: flex;
  user-select: none;

  line-height: 1.2em;

  margin: 0 0 4px 0;

  > .info {
    flex-grow: 1;
    min-width: 0;

    padding: 0.5em 0.5em calc(0.5em + #{$ore-strip}) 0.5em;

    @include ore-button;

    > .marker-set-switch {
      position: relative;

      .label {
        margin: 0 3em 0 0;
        overflow: hidden;
        text-overflow: ellipsis;
      }

      > .switch {
        position: absolute;
        top: 0;
        right: 0;
      }
    }

    > .stats {
      display: flex;
      font-size: 0.8em;
      color: var(--theme-fg-light);

      > div {
        &:not(:first-child) {
          margin-left: 0.5em;
          padding-left: 0.5em;
          border-left: solid $ore-border-width var(--ore-border);
        }
      }
    }
  }

  > .open-menu-button {
    position: relative;
    width: 2em;
    flex-shrink: 0;
    margin-left: -$ore-border-width;

    &.active {
      @include ore-button;

      > svg {
        position: absolute;
        fill: var(--theme-fg);

        top: calc(50% - #{$ore-strip * 0.5});
        width: 32px;
        height: 32px;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    }

    &:not(.active) {
      display: none;
    }
  }
}
</style>
