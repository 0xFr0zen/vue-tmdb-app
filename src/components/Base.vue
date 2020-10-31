<template>
  <h1>API-SetFilter</h1>
  <button @click="setMovieScary('scary')">movie scary</button>
  <button @click="setMovieScary('not scary')">movie not scary</button>
  <br />
  <button @click="setMovieModern('new')">new school</button>
  <button @click="setMovieModern('old')">old but gold</button>
  <br />
  <button @click="setMoviePopularity(1)">POPULAR 1</button>
  <button @click="setMoviePopularity(2)">POPULAR 2</button>
  <button @click="setMoviePopularity(3)">POPULAR 3</button>
  <br />

  <button @click="getResults">GET RESULTS</button>
  <div id="results">
    <div class="image-wrapper">
      <img
        v-for="r in result"
        v-bind:key="r.id"
        :src="getImageUrl(r.poster_path)"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Base',
  data() {
    return {
      params: {
        api_key: process.env.API_KEY,
        sort_by: 'popularity.desc',
        include_adult: false,
        'vote_average.gte': 10,
        'primary_release_date.gte': '2020-10-10',
        with_genre: 27
      },
      IMG_URL: 'https://image.tmdb.org/t/p/w440_and_h660_face/',
      URL: 'https://api.themoviedb.org/3/discover/movie',
      result: {}
    };
  },
  methods: {
    setFilter(name, value) {
      this.params[name] = value;
    },
    /**
     *	How Modern should the movie be?
     *	@param which `new/old`
     */
    setMovieModern(which) {
      let year = new Date().getFullYear();
      if (which === 'new') {
        year = year - 2;
      }
      if (which === 'old') {
        year = year - 10;
      }
      this.setFilter('primary_release_date.gte', `${year}-01-01`);
    },
    /**
     *  mode can be `true/false`
     *
     *	@param mode `true/false` sets the scary mode
     */
    setMovieScary(mode) {
      if (mode === 'scary') {
        this.setFilter('without_genres', '');
        this.setFilter('with_genres', '27,35,10751');
      } else {
        this.setFilter('with_genres', '27,35');
        this.setFilter('without_genres', '10751');
      }
    },
    /**
     *  Popularity can be 1, 2, 3
     *
     *	@param mode `true/false` sets the scary mode
     */
    setMoviePopularity(popular) {
      let max = 10;
      let split = 3;
      let scaler = max / split;
      switch (popular) {
        case 1:
          this.setFilter('vote_average.gte', scaler);
          this.setFilter('vote_average.lte', scaler * (popular + 1));
          break;
        case 2:
          this.setFilter('vote_average.gte', scaler * popular);
          this.setFilter('vote_average.lte', scaler * (popular + 1));
          break;
        case 3:
          this.setFilter('vote_average.gte', scaler * (popular - 1));
          this.setFilter('vote_average.lte', 10);
          break;
        default:
          break;
      }
    },
    getResults() {
      axios.get(this.URL, { params: this.params }).then(response => {
        this.result = response.data.results;
        console.log(this.result);
      });
    },
    getImageUrl(url) {
      return `${this.IMG_URL}${url}`;
    }
  }
};
</script>
<style scoped>
.genre {
  width: auto;
  height: auto;
  background-color: lightblue;
  padding: 2rem;
  margin-bottom: 2rem;
}
</style>
