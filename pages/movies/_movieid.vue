<template>
  <Loading v-if="$fetchState.pending"/>
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }">Retour</NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" :alt="`Image du film ${ movie.title }`">
      </div>
      <div class="movie-content">
        <h1>Titre : {{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>TagLine : </span>"{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>Soritie le :</span>
          {{
            new Date(movie.release_date).toLocaleString('fr-FR', {
              month: 'long',
              day: 'numeric',
              year: 'numeric'
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Durée :</span> {{ movie.runtime }} minutes.
        </p>
        <p class="movie-fact">
          <span>Revenus :</span>
          {{
            movie.revenue.toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD'
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Résumé :</span> {{ movie.overview }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
// axios import
import axios from 'axios';
// Component import
import Loading from "../../components/Loading";
export default {
  name: "SingleMovie",
  components: {
    Loading
  },
  data() {
    return {
      movie: ''
    }
  },
  async fetch() {
    await this.getSingleMovie();
  },
  fetchDelay: 1000,
  head() {
    return {
      title: this.movie.title,
    }
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=bf2aa90d91269270e8af9e5fe271122d&language=fr&page=1`);
      const result = await data;
      this.movie = result.data;
    }
  }
}
</script>

<style lang="scss" scoped>
.single-movie {
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
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
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
