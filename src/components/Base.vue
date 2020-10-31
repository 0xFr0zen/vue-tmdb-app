<template>
  <h1>API-SetFilter</h1>
  <div class="genre" @click="setFilter('with_genre', 27)">
    CLICK HERE TO SET FILTER 'GENRE'
  </div>
  <button value="LOL" @click="getResults">GET RESULTS</button>
  <div id="results">
    {{ result }}
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Base',
  data() {
    return {
      params: {
        api_key: 'API_KEY_HERE'
      },
      URL: 'https://api.themoviedb.org/3/discover/movie',
      result: {}
    };
  },
  methods: {
    setFilter(name, value) {
      this.params[name] = value;
    },
    getResults() {
      axios.get(this.URL, { params: this.params }).then(response => {
        this.result = response.data.results;
        console.log(this.result);
      });
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
