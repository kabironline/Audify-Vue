<template>
  <section class="section section-genre">
    <h2 class="heading-2">Genres</h2>
    <TileGenre :genres="genres" />
  </section>
  <section class="section section-latest-track">
    <h2 class="heading-2 heading-link" @click.prevent="goToNewReleases">New Releases</h2>
    <CarouselTrack :tracks="latestTracks" />
  </section>
  <section class="section section-latest-album">
    <h2 class="heading-2 heading-link" @click.prevent="goToNewAlbumReleases">Latest Albums</h2>
    <CarouselAlbum :albums="latestAlbums" />
  </section>
  <section class="section section-top-track">
    <h2 class="heading-2 heading-link" @click.prevent="goToTopTracks">Top Tracks</h2>
    <TileTrack :tracks="topTracks" />
  </section>
  <section class="section section-top-track">
    <h2 class="heading-2 heading-link" @click.prevent="goToTopRatedTracks">Top Rated Tracks</h2>
    <TileTrack :tracks="topRatedTracks" />
  </section>
  <section class="section section-top-channel">
    <h2 class="heading-2">Top Channels</h2>
    <TileChannel :channels="topChannels" />
  </section>
</template>

<script>
import TileGenre from '@/components/TileGenre.vue'
import TileTrack from '@/components/TileTrack.vue'
import TileChannel from '@/components/TileChannel.vue'
import CarouselTrack from '@/components/CarouselTrack.vue'
import CarouselAlbum from '@/components/CarouselAlbum.vue'
import {
  getTopChannels,
  getTopTracks,
  getLatestTracks,
  getGenres,
  getTopRatedTracks,
  getLatestAlbums
} from '@/helper/getters'
export default {
  name: 'ExploreView',
  components: {
    TileGenre,
    TileTrack,
    TileChannel,
    CarouselTrack,
    CarouselAlbum
  },
  data: () => ({
    latestTracks: [],
    latestAlbums: [],
    topTracks: [],
    topRatedTracks: [],
    topChannels: [],
    genres: [],
    loading: true
  }),
  methods: {
    goToTopTracks() {
      this.$router.push('/top/tracks')
    },
    goToNewReleases() {
      this.$router.push('/new-releases')
    },
    goToNewAlbumReleases() {
      this.$router.push('/new-album-releases')
    },
    goToTopRatedTracks() {
      this.$router.push('/top/tracks/ratings')
    },
    async getExploreData() {
      getGenres().then((genres) => {
        this.genres = genres
      })
      getLatestTracks().then((latestTracks) => {
        this.latestTracks = latestTracks
      })
      getTopTracks(16).then((topTracks) => {
        this.topTracks = topTracks
      })
      getTopRatedTracks(16).then((topRatedTracks) => {
        this.topRatedTracks = topRatedTracks
      })
      getTopChannels(16).then((topChannels) => {
        this.topChannels = topChannels
      })
      getLatestAlbums().then((latestAlbums) => {
        this.latestAlbums = latestAlbums
      })
    }
  },
  async mounted() {
    await this.getExploreData()
  }
}
</script>

<style></style>
@/helper/getters
