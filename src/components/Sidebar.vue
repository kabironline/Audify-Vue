<template>
  <div class="sidebar">
    <div class="sidebar__top" @click="navigateToHome">
      <div class="sidebar-logo">&nbsp;</div>
    </div>
    <hr class="sidebar__hr" />
    <div class="side-nav">
      <SidebarListItem v-for="item in menuItems" :key="item.title" :item="item" />
    </div>

    <hr class="sidebar__hr" />
    <BtnAction icon="add" text="Create Playlist" color="white" @click="openPlaylistModal" />
    <br />
    <ul class="side-nav" v-if="showPlaylist">
      <TilePlaylist :playlists="playlists" />
    </ul>
  </div>
</template>

<script>
import { useUserStore } from '@/stores/user'
import BtnAction from './BtnAction.vue'
import SidebarListItem from './SidebarListItem.vue'
import { mapState } from 'pinia'
import TilePlaylist from './TilePlaylist.vue'
import { useModalStore } from '@/stores/modal'
import { mapActions } from 'pinia'
import { toRaw } from 'vue'
export default {
  name: 'AppSidebar',
  components: {
    SidebarListItem,
    BtnAction,
    TilePlaylist
  },
  data: () => ({
    drawer: false,
    playlistModalVisible: false,
    menuItems: [],
    playlists: [],
    isLoading: false,
    flipFlop: false
  }),
  computed: {
    ...mapState(useUserStore, ['getUserChannel', 'getUserId', 'getUserPlaylist']),
    showPlaylist() {
      return this.isLoading ? true : this.playlists ? this.playlists.length > 0 : false
    }
  },
  methods: {
    ...mapActions(useModalStore, ['openPlaylistModal']),
    navigateToHome() {
      this.$router.push('/')
    },
    updateMenuItems() {
      const menuItems = [
        { title: 'Home', icon: 'home', link: '/', route_name: 'home' },
        { title: 'Explore', icon: 'explore', link: '/explore', route_name: 'explore' },
        {
          title: 'Dashboard',
          icon: 'dashboard',
          link: `/dashboard/${this.getUserId}`,
          route_name: 'dashboard'
        }
      ]
      if (this.getUserChannel !== null) {
        menuItems.push({
          title: 'Channel Dashboard',
          link: `/channel/${this.getUserChannel.id}/dashboard`,
          icon: 'music_note',
          route_name: 'channel-dashboard'
        })
        menuItems.push({
          title: 'Upload Track',
          icon: 'upload',
          link: '/upload',
          route_name: 'track-upload'
        })
        menuItems.push({
          title: 'Create Album',
          icon: 'album',
          link: '/album/create',
          route_name: 'album-create'
        })
      }
      this.menuItems = menuItems
    }
  },
  mounted() {
    this.updateMenuItems()
    this.playlists = this.getUserPlaylist
    this.isLoading = false

    this.$watch(
      () => toRaw(this.getUserPlaylist),
      () => {
        this.playlists = this.getUserPlaylist
      }
    )

    this.$watch(
      () => toRaw(this.getUserChannel),
      () => {
        this.updateMenuItems()
      }
    )
  }
}
</script>

<style scoped>
.sidebar {
  overflow-y: auto;
  overflow-x: hidden;
  width: 30rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-right: 1px solid var(--color-border-light);
}

.sidebar__top {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  font-size: 2.5rem;
}

.sidebar-logo {
  width: 15rem;
  height: 11rem;
  mask-image: url(@/assets/images/logo.svg);
  mask-size: contain;
  mask-repeat: no-repeat;
  -webkit-mask-size: contain;
  -webkit-mask-image: url(@/assets/images/logo.svg);
  -webkit-mask-repeat: no-repeat;
  background-color: var(--color-primary);
}

.sidebar__hr {
  margin: 2rem 0;
  width: 100%;
  border: none;
  border-bottom: 1px solid var(--color-border-light);
}

.side-nav {
  list-style: none;
  width: 100%;
}
</style>
