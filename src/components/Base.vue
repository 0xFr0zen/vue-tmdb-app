<template>
  <h1>sdas</h1>
  <div class="genre" @click="setFilter('with_genre', 27)" />
  <button value="LOL" @click="getResults">GET RESULTS</button>
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
  width: 2rem;
  height: 2rem;
  background-color: red;
}
</style>
