<template>
  <div class="marker-item" :class="{'marker-hidden': !marker.visible}">
    <div class="marker-button" :title="marker.id" @click="click(false)">
      <div class="icon" v-if="marker.type === 'player'">
        <img :src="'maps/' + mapId +  '/assets/playerheads/' + marker.playerUuid + '.png'" alt="playerhead" @error="steve">
      </div>
      <div class="info">
        <div class="label">{{markerLabel}}</div>
        <div class="stats">
          <div v-if="appState.debug">
            {{marker.type}}-marker
          </div>
          <div>
            ({{ position(marker.position.x) }} | {{ position(marker.position.y) }} | {{ position(marker.position.z) }})
          </div>
        </div>
      </div>
    </div>
    <div class="follow-player-button" :class="{active: controls.controls.followingPlayer && controls.controls.followingPlayer.id === marker.id}"
         v-if="marker.type === 'player'" @click="click(true)" :title="$t('markers.followPlayerTitle')">
      <svg viewBox="0 0 16 16" shape-rendering="crispEdges"><path d="M7 0h2v1h-2zM7 1h2v1h-2zM5 2h6v1h-6zM4 3h2v1h-2zM10 3h2v1h-2zM3 4h2v1h-2zM11 4h2v1h-2zM2 5h2v1h-2zM12 5h2v1h-2zM2 6h1v1h-1zM7 6h2v1h-2zM13 6h1v1h-1zM0 7h3v1h-3zM6 7h4v1h-4zM13 7h3v1h-3zM0 8h3v1h-3zM6 8h4v1h-4zM13 8h3v1h-3zM2 9h1v1h-1zM7 9h2v1h-2zM13 9h1v1h-1zM2 10h2v1h-2zM12 10h2v1h-2zM3 11h2v1h-2zM11 11h2v1h-2zM4 12h2v1h-2zM10 12h2v1h-2zM5 13h6v1h-6zM7 14h2v1h-2zM7 15h2v1h-2z"/></svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "MarkerItem",
  props: {
    marker: Object,
  },
  data() {
    return {
      appState: this.$bluemap.appState,
      controls: this.$bluemap.mapViewer.controlsManager.data,
      mapId: this.$bluemap.mapViewer.data.map.id
    }
  },
  computed: {
    markerLabel() {
      switch (this.marker.type) {
        case "player" : return this.marker.name;
        default : break;
      }

      if (this.marker.label){
        let strippedLabel = /^(?:<[^>]*>\s*)*([^<>]*\S[^<>]*)(?:<|$)/gi.exec(this.marker.label);
        if (strippedLabel && strippedLabel.length > 1) {
          return strippedLabel[1];
        }
      }

      return this.marker.id;
    },
    position() {
      return (v) => {
        return Math.floor(v);
      }
    }
  },
  methods: {
    async click(follow) {
      let cm = this.$bluemap.mapViewer.controlsManager;
      
      if (cm.controls && cm.controls.stopFollowingPlayerMarker) {
        cm.controls.stopFollowingPlayerMarker();
      }

      if (this.marker.type === "player") {

        if (this.marker.foreign) {
          let matchingMap = await this.$bluemap.findPlayerMap(this.marker.playerUuid);
          if (!matchingMap) return;

          //this.$bluemap.appState.menu.closeAll();
          await this.$bluemap.switchMap(matchingMap.data.id);
        }

        if (follow && cm.controls && cm.controls.followPlayerMarker && this.marker.visible) {
          cm.controls.followPlayerMarker(this.marker);
        }

      } else if (!this.marker.visible) {
        return;
      }

      cm.position.copy(this.marker.position);
    },
    steve(event) {
      event.target.src = "assets/steve.png";
    }
  }
}
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

.side-menu .marker-item {
  display: flex;
  white-space: nowrap;
  user-select: none;

  line-height: 1.2em;

  margin: 0 0 4px 0;

  &.marker-hidden {
    opacity: 0.5;
    filter: grayscale(1);
  }

  .marker-button {
    display: flex;
    flex-grow: 1;
    min-width: 0;

    padding-bottom: $ore-strip;

    @include ore-button;

    > .info {
      flex-grow: 1;
      min-width: 0;

      padding: 0.5em;

      .label {
        overflow: hidden;
        text-overflow: ellipsis;
      }

      .stats {
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

    > .icon {
      height: 2.5em;
      margin: 0.5em 0 0.5em 0.5em;
      flex-shrink: 0;

      img {
        image-rendering: pixelated;
        height: 100%;
        box-sizing: border-box;
        border: solid $ore-border-width var(--ore-border);
      }
    }
  }

  > .follow-player-button {
    position: relative;
    width: 2em;
    flex-shrink: 0;
    margin-left: -$ore-border-width;

    @include ore-button;

    &.active {
      @include ore-button-accent;

      > svg {
        fill: var(--ore-accent-fg);
      }
    }

    > svg {
      position: absolute;
      left: 0;
      width: 100%;
      fill: var(--theme-fg);

      top: calc(50% - #{$ore-strip * 0.5});
      transform: translate(0, -50%) scale(0.75);
    }
  }
}
</style>
