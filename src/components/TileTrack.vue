<template>
  <div class="track-tiles" link>
    <div class="track-tile" v-for="n in 16" :key="n">
      <div class="track-tile__position">
        <p class="track-tile__position--text">{{ n }}</p>
      </div>
      <div class="track-tile__cover">
        <a href="/player" class="">
          <div>
            <img
              :src="`https://picsum.photos/300/200?random=${n}`"
              alt="track Image"
              class="track-tile__cover--img"
            />
            <i class="fas fa-play track-tile__cover--playbutton"></i>
          </div>
        </a>
      </div>
      <div class="track-tile__text">
        <a href="" class="track-tile__text--title">Name of track {{ n }}</a>
        <a href="" class="track-tile__text--artist">Channel Name {{ n }}</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TileTrack'
}
</script>

<style scoped>
/* track Tile */
.track-tiles {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(30rem, 1fr));
  padding: 2rem 0;
}
.track-tile {
  display: grid;
  grid-template-columns: min-content min-content 1fr;
  grid-gap: 1rem;
  align-items: center;
  text-decoration: none;
  color: white;
  font-size: 1.8rem;
  padding: 1rem 2rem;
  border-radius: 5px;
  transition: all 0.2s ease-out;

  &:hover {
    background-color: var(--color-hover);
  }

  &:hover {
    .track-tile__cover--playbutton {
      opacity: 1;
      font-size: 3rem;
    }

    .track-tile__cover--img {
      filter: brightness(0.5);
    }
    .track-tile__cover--img.selectable {
      filter: brightness(1);
    }
  }
}

.track-tile-small {
  grid-template-columns: min-content 1fr;
}

.track-tile-checkbox:checked + .track-tile-label .track-tile {
  background-color: var(--color-hover);
}

.track-tile__position {
  font-size: 1.8rem;
  font-weight: bold;
  /* color: var(--text-label-color); */
}

.track-tile__cover {
  /* width: 8.9rem; */
  height: 6rem;
  position: relative;
  overflow: hidden;
  border-radius: 5px;
  aspect-ratio: 16/9;
}

.track-tile__cover--img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.track-tile__cover--playbutton {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 2rem;
  color: white;
  opacity: 0;
  transition: all 0.2s ease-out;
  /* transform: scale(0.8); */
  transform-origin: center;
}

.track-tile__text {
  display: flex;
  gap: 1rem;
  flex-direction: column;
  align-items: flex-start;
  text-overflow: ellipsis;
  overflow: hidden;

  .track-tile__text--title {
    font-weight: bold;
    font-size: 1.6rem;
    text-decoration: none;
    color: white;
    display: block;
    text-wrap: nowrap;
    text-overflow: ellipsis;

    &:hover {
      text-decoration: underline;
    }
  }

  .track-tile__text--artist {
    font-weight: lighter;
    font-size: 1.4rem;
    text-decoration: none;
    color: var(--text-subtitle-color);
    &:hover {
      text-decoration: underline;
    }
  }
}
</style>