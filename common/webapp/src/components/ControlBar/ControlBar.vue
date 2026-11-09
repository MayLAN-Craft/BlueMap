<template>
  <div class="control-bar">
    <MenuButton :close="appState.menu.isOpen" :back="false" @action="appState.menu.reOpenPage()" :title="$t('menu.tooltip')" />
    <div class="space thin-hide"></div>
    <SvgButton v-if="appState.maps.length > 1" class="thin-hide" :title="$t('maps.tooltip')"
               @action="appState.menu.openPage('maps', $t('maps.title'))">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M1 2h4v1h-4zM11 2h4v1h-4zM1 3h4v1h-4zM6 3h4v1h-4zM11 3h4v1h-4zM1 4h4v1h-4zM6 4h1v1h-1zM9 4h1v1h-1zM11 4h4v1h-4zM1 5h4v1h-4zM6 5h1v1h-1zM9 5h1v1h-1zM11 5h4v1h-4zM1 6h4v1h-4zM6 6h1v1h-1zM9 6h1v1h-1zM11 6h4v1h-4zM1 7h4v1h-4zM6 7h1v1h-1zM9 7h1v1h-1zM11 7h4v1h-4zM1 8h4v1h-4zM6 8h1v1h-1zM9 8h1v1h-1zM11 8h4v1h-4zM1 9h4v1h-4zM6 9h1v1h-1zM9 9h1v1h-1zM11 9h4v1h-4zM1 10h4v1h-4zM6 10h1v1h-1zM9 10h1v1h-1zM11 10h4v1h-4zM1 11h4v1h-4zM6 11h1v1h-1zM9 11h1v1h-1zM11 11h4v1h-4zM1 12h4v1h-4zM6 12h1v1h-1zM9 12h1v1h-1zM11 12h4v1h-4zM6 13h4v1h-4z"/></svg>
    </SvgButton>
    <SvgButton v-if="showMapMenu && showMarkerMenu" class="thin-hide" :title="$t('markers.tooltip')"
               @action="appState.menu.openPage('markers', $t('markers.title'), {markerSet: markers})">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M5 2h6v1h-6zM3 3h10v1h-10zM2 4h5v1h-5zM9 4h5v1h-5zM2 5h4v1h-4zM10 5h4v1h-4zM2 6h4v1h-4zM10 6h4v1h-4zM2 7h5v1h-5zM9 7h5v1h-5zM3 8h10v1h-10zM3 9h10v1h-10zM4 10h8v1h-8zM5 11h6v1h-6zM6 12h4v1h-4zM7 13h2v1h-2z"/></svg>
    </SvgButton>
    <SvgButton v-if="showMapMenu && !playerMarkerSet.fake" class="thin-hide" :title="$t('players.tooltip')" @action="openPlayerList">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M6 2h4v1h-4zM6 3h4v1h-4zM1 4h3v1h-3zM6 4h4v1h-4zM12 4h3v1h-3zM1 5h3v1h-3zM6 5h4v1h-4zM12 5h3v1h-3zM1 6h3v1h-3zM12 6h3v1h-3zM5 7h6v1h-6zM0 8h4v1h-4zM5 8h6v1h-6zM12 8h4v1h-4zM0 9h4v1h-4zM5 9h6v1h-6zM12 9h4v1h-4zM0 10h4v1h-4zM5 10h6v1h-6zM12 10h4v1h-4zM0 11h4v1h-4zM5 11h6v1h-6zM12 11h4v1h-4zM5 12h6v1h-6zM5 13h6v1h-6z"/></svg>
    </SvgButton>
    <div class="space thin-hide greedy"></div>
    <DayNightSwitch v-if="showMapMenu" class="thin-hide" :title="$t('lighting.dayNightSwitch.tooltip')" />
    <div class="space thin-hide"></div>
    <ControlsSwitch v-if="showMapMenu && showViewControls" class="thin-hide"></ControlsSwitch>
    <div class="space thin-hide" v-if ="showViewControls"></div>
    <SvgButton v-if="showMapMenu" class="thin-hide" :title="$t('resetCamera.tooltip')" @action="$bluemap.resetCamera()">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M3 2h2v1h-2zM3 3h11v1h-11zM3 4h11v1h-11zM3 5h10v1h-10zM3 6h9v1h-9zM3 7h10v1h-10zM3 8h11v1h-11zM3 9h2v1h-2zM3 10h2v1h-2zM3 11h2v1h-2zM3 12h2v1h-2zM3 13h2v1h-2z"/></svg>
    </SvgButton>
    <PositionInput v-if="showMapMenu" class="pos-input" />
    <Compass v-if="showMapMenu" :title="$t('compass.tooltip')" />
  </div>
</template>

<script>
  import PositionInput from "./PositionInput.vue";
  import Compass from "./Compass.vue";
  import DayNightSwitch from "./DayNightSwitch.vue";
  import ControlsSwitch from "./ControlsSwitch.vue";
  import MenuButton from "./MenuButton.vue";
  import SvgButton from "./SvgButton.vue";

  export default {
    name: "ControlBar",
    components: {
      SvgButton,
      MenuButton,
      ControlsSwitch,
      DayNightSwitch,
      PositionInput,
      Compass
    },
    data() {
      return {
        appState: this.$bluemap.appState,
        markers: this.$bluemap.mapViewer.markers.data,
        mapViewer: this.$bluemap.mapViewer.data
      }
    },
    computed: {
      playerMarkerSet() {
        for (let set of this.markers.markerSets) {
          if (set.id === "bm-players") return set;
        }

        return {
          id: "bm-players",
          label: "Players",
          markerSets: [],
          markers: [],
          fake: true,
        }
      },
      showMapMenu() {
        return this.mapViewer.mapState === "loading" || this.mapViewer.mapState === "loaded";
      },
      showViewControls() {
        if (!this.mapViewer.map) return false;
        return this.mapViewer.map.views.length > 1;
      },
      showMarkerMenu() {
        return this.hasMarkers(this.markers)
      }
    },
    methods: {
      openPlayerList() {
        let playerList = this.playerMarkerSet;
        this.appState.menu.openPage('markers', this.$t("players.title"), {markerSet: playerList});
      },
      hasMarkers(markerSet) {
        if (markerSet.markers.length > 0) return true;
        for (let set of markerSet.markerSets) {
          if (set.id !== "bm-players" && set.id !== "bm-popup-set") {
            if (this.hasMarkers(set)) return true;
          }
        }
        return false;
      }
    }
  }
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

  .control-bar {
    position: fixed;
    top: 0;
    left: 0;

    display: flex;
    gap: 4px;

    @include ore-drop-shadow;
    height: 2em;

    margin: 0.5em;
    width: calc(100% - 1em);

    .pos-input {
      max-width: 20em;
      width: 100%;
    }

    .space {
      width: 0.25em;
      flex-shrink: 0;

      &.greedy {
        flex-grow: 1;
      }
    }

    @media (max-width: $mobile-break) {
      margin: 0;
      width: 100%;
      gap: 0;

      filter: none;
      background-color: var(--theme-bg);
      border-bottom: solid $ore-border-width var(--ore-border);

      > :not(:first-child) {
        margin-left: -2px;
      }

      .pos-input {
        max-width: unset;
      }

      .thin-hide {
        display: none;
      }

      .space {
        width: 0;
      }
    }

  }
</style>
