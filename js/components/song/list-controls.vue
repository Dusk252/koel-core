<template>
  <div class="song-list-controls">
    <btn-group uppercased>
      <template v-if="fullConfig.play">
        <btn
          @click.prevent="playAll"
          class="btn-play-all"
          highlight
          title="Play all all"
          v-if="selectedSongs.length < 2 && songs.length"
        >
          <i class="fa fa-play"></i> All
        </btn>

        <btn
          @click.prevent="playSelected"
          class="btn-play-selected"
          highlight
          title="Play selected"
          v-if="selectedSongs.length > 1"
        >
          <i class="fa fa-play"></i> Selected
        </btn>
      </template>
      <template v-if="fullConfig.shuffle">
        <btn
          @click.prevent="shuffleAll"
          class="btn-shuffle-all"
          highlight
          title="Shuffle all"
          v-if="selectedSongs.length < 2 && songs.length"
        >
          <i class="fa fa-random"></i> All
        </btn>

        <btn
          @click.prevent="shuffleSelected"
          class="btn-shuffle-selected"
          highlight
          title="Shuffle selected"
          v-if="selectedSongs.length > 1"
        >
          <i class="fa fa-random"></i> Selected
        </btn>
      </template>

      <btn
        :title="`${showingAddToMenu ? 'Cancel' : 'Add selected songs to…'}`"
        @click.prevent.stop="showingAddToMenu = !showingAddToMenu"
        class="btn-add-to"
        green
        v-if="selectedSongs.length"
      >
        {{ showingAddToMenu ? 'Cancel' : 'Add To…' }}
      </btn>

      <btn
        @click.prevent="clearQueue"
        class="btn-clear-queue"
        red
        v-if="showClearQueueButton"
        title="Clear current queue"
      >
        Clear
      </btn>

      <btn
        @click.prevent="deletePlaylist"
        class="del btn-delete-playlist"
        red
        title="Delete this playlist"
        v-if="showDeletePlaylistButton"
      >
        <i class="fa fa-times"></i> Playlist
      </btn>

    </btn-group>

    <add-to-menu
      @closing="closeAddToMenu"
      :config="fullConfig.addTo"
      :songs="selectedSongs"
      :showing="showingAddToMenu"
      v-koel-clickaway="closeAddToMenu"
    />
  </div>
</template>

<script>

export default {
  props: {
    config: Object,
    songs: {
      type: Array,
      default: () => []
    },
    selectedSongs: {
      type: Array,
      default: () => []
    }
  },

  components: {
    AddToMenu: () => import('./add-to-menu'),
    Btn: () => import('@/components/ui/btn'),
    BtnGroup: () => import('@/components/ui/btn-group')
  },

  data: () => ({
    fullConfig: {
      play: true,
      shuffle: true,
      addTo: {
        queue: true,
        favorites: true,
        playlists: true,
        newPlaylist: true
      },
      clearQueue: false,
      deletePlaylist: false
    },
    showingAddToMenu: false,
    numberOfQueuedSongs: 0
  }),

  computed: {
    showClearQueueButton () {
      return this.fullConfig.clearQueue
    },

    showDeletePlaylistButton () {
      return this.fullConfig.deletePlaylist
    }
  },

  created () {
    this.fullConfig = Object.assign(this.fullConfig, this.config)
  },

  methods: {
    shuffleAll () {
      this.$emit('playAll', true)
    },

    shuffleSelected () {
      this.$emit('playSelected', true)
    },

    playAll () {
      this.$emit('playAll', false)
    },

    playSelected () {
      this.$emit('playSelected', false)
    },

    clearQueue () {
      this.$emit('clearQueue')
    },

    deletePlaylist () {
      this.$emit('deletePlaylist')
    },

    closeAddToMenu () {
      this.showingAddToMenu = false
    }
  }
}
</script>

<style lang="scss" scoped>
.song-list-controls {
  position: relative;
  min-width: 196px;

  @media only screen and (max-width: 768px) {
    min-width: 0;
  }
}
</style>