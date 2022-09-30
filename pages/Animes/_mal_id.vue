<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-anime">
    <nuxt-link class="button" :to="{ name: 'index' }"> Back </nuxt-link>
    <div class="anime-info">
      <div class="anime-img">
        <img :src="anime.data.images.jpg.image_url" alt="poster" />
      </div>

      <div class="anime-content">
        <h1>{{ anime.data.title }}</h1>
        <h2>{{ anime.data.title_japanese }}</h2>
        <h3>Status : {{ anime.data.status }}</h3>
        <h3>Episode : {{ anime.data.episodes }}</h3>
        <h3>Season: {{ anime.data.season }}</h3>
        <h3>Year: {{ anime.data.year }}</h3>
        <h3>Type: {{ anime.data.type }}</h3>
        <h3>Popularity: {{ anime.data.popularity }}</h3>
        <h3>Rank: {{ anime.data.rank }}</h3>

        <p class="anime-fact">
          Synopsis : <br />
          {{ anime.data.synopsis }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'single-anime',
  middleware: 'auth',
  data() {
    return {
      anime: null,
    }
  },

  async fetch() {
    await this.getSingleAnime()
  },
  fetchDelay: 1000,

  methods: {
    async getSingleAnime() {
      const data = axios.get(
        `https://api.jikan.moe/v4/anime/${this.$route.params.mal_id}`
      )
      const result = await data
      this.anime = result.data
      console.log(this.anime)
    },
  },
}
</script>

<style lang="scss">
.single-anime {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .anime-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .anime-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .anime-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .anime-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
