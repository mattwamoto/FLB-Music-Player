<template>
  <div class="tab RecentsTab scroll_y">
    <div
      v-if="recentlyPlayedTracks.length === 0"
      class="centerContents"
      style="height: 100%"
    >
      <img width="300px" src="@img/no_recents.svg" />
      <p style="font-family: inherit">You still haven't played anything</p>
    </div>
    <div @click="addTracksToQueue">
      <track-card
        v-for="track in recentlyPlayedTracks"
        :key="track.fileLocation"
        :source="track"
        :index="0"
      />
    </div>
  </div>
</template>
<script>
import { sortArrayOfObjects } from '@/shared-utils';
import { mapMutations } from 'vuex';
export default {
  name: 'RecentsTab',
  data() {
    return {};
  },
  watch: {
    flipSortOrder() {
      //Make It Work
      this.recentlyPlayedTracks.reverse();
    }
  },
  computed: {
    recentlyPlayedTracks() {
      const sortParameter = this.$store.state.sortParameter;
      const tracks = [
        ...this.$store.state.TabsManager.tabsData.recentlyPlayedTracks
      ];
      sortArrayOfObjects(tracks, sortParameter);
      return tracks;
    },
    flipSortOrder() {
      return this.$store.state.flipSortOrder;
    }
  },
  methods: {
    ...mapMutations(['overWriteCustomQueue']),
    addTracksToQueue() {
      this.overWriteCustomQueue(this.recentlyPlayedTracks);
    }
  }
};
</script>

<style>
.RecentsTab {
  overflow: hidden;
  overflow-y: scroll;
  height: 100%;
  padding-right: 10px;
}
</style>
