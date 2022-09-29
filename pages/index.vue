<template>
  <div class="home">
    <Hero />

    <!-- Animek Grid -->

    <div class="container movies">
      <div id="movie-grid" class="movies-grid">
        <div class="movie" v-for="(anime, index) in Animek" :key="index">
          <div class="movie-img">
            <img :src="anime.images.jpg.image_url" alt="poster" />
            <p class="review">{{ anime.score }}</p>
            <p class="overview">
              {{ anime.synopsis.slice(0, 500) }}
              <span v-if="anime.synopsis.length > 500">...</span>
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{ anime.title.slice(0, 75) }}
              <span v-if="anime.title.length > 75">...</span>
            </p>
            <p class="title">
              {{ anime.title_japanese.slice(0, 75) }}
              <span v-if="anime.title.length > 75">...</span>
            </p>
            <NuxtLink
              class="button button-light"
              :to="{ name: 'Animes-mal_id', params: { mal_id: anime.mal_id } }"
            >
              Details
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Hero from '../components/Hero.vue'

export default {
  data() {
    return {
      Animek: [],
    }
  },
  async fetch() {
    await this.getTopAnime()
  },
  methods: {
    async getTopAnime() {
      const data = axios.get('https://api.jikan.moe/v4/top/anime')
      const result = await data
      result.data.data.forEach((anime) => {
        this.Animek.push(anime)
      })
      console.log(this.Animek)
    },
  },
  name: 'IndexPage',
  components: { Hero },
}
</script>

<style lang="scss" scoped>
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 32px 16px;
    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #c92502;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(201, 38, 2, 0.9);
            padding: 12px;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
          }
        }
        .info {
          margin-top: auto;
          .title {
            margin-top: 8px;
            color: #fff;
            font-size: 20px;
          }
          .release {
            margin-top: 8px;
            color: #c9c9c9;
          }
          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>
