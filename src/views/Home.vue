<template>
  <div class="movie">
    <form @submit.prevent="showMovie()">
      <input class="search" type="text" v-model="searchQuery" placeholder="search movies" />
    </form>

    <div class="show-movie">
      <h1>{{ movie.Title }}</h1>
      <img :src="`${movie.Poster}`" alt="" />
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      searchQuery: "",
      movie: {},
    };
  },
  created: function () {},
  mounted: function () {},
  methods: {
    showMovie: function () {
      axios.get(`/movies/${this.searchQuery}`).then((response) => {
        console.log("Show movie", response);
        this.movie = response.data;
      });
    },
  },
};
</script>
