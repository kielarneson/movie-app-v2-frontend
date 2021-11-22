<template>
  <div class="show-movie">
    <form @submit.prevent="showMovie()">
      <input class="search" type="text" v-model="searchQuery" placeholder="search movies" />
    </form>

    <div>
      <h1>{{ movie.Title }}</h1>
      <img :src="`${movie.Poster}`" alt="" />
    </div>
  </div>
</template>

<style>
.search {
  text-align: center;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      searchQuery: "",
      movie: {},
      actionMovie: { movie: {} },
    };
  },
  created: function () {},
  mounted: function () {},
  methods: {
    showMovie: function () {
      axios.get(`/movies/${this.searchQuery}`).then((response) => {
        console.log("Show movie", response);
        this.movie = response.data;
        this.searchQuery = "";

        // This only creates a new movie if "Action" is included in the genre
        if (this.movie.Genre.includes("Action") === true) {
          var newActionMovieParams = {
            title: this.movie.Title,
            imdb_id: this.movie.imdbID,
          };

          axios.post("/action_movies", newActionMovieParams).then((response) => {
            console.log("Create Action movie", response);
            this.actionMovie = response.data;

            var newMovieParams = {
              title: this.movie.Title,
              year: this.movie.Year,
              rated: this.movie.Rated,
              released: this.movie.Released,
              runtime: this.movie.Runtime,
              formatted_runtime: this.movie.formatted_runtime,
              actors: this.movie.Actors,
              awards: this.movie.Awards,
              box_office: this.movie.BoxOffice,
              director: this.movie.Director,
              genre: this.movie.Genre,
              language: this.movie.Language,
              plot: this.movie.Plot,
              poster: this.movie.Poster,
              entertainment_type: this.movie.Type,
              writer: this.movie.Writer,
              imdb_id: this.movie.imdbID,
              imdb_rating: this.movie.imdbRating,
              imdb_votes: this.movie.imdbVotes,
              internet_movie_database_rating: this.movie.Ratings[0].Value,
              rotten_tomatoes_rating: this.movie.Ratings[1].Value,
              metacritic_rating: this.movie.Ratings[2].Value,
              action_movie_id: this.actionMovie.movie.id,
            };
            axios.post("/movies", newMovieParams).then((response) => {
              console.log("Create movie", response);
            });
          });
        }
      });
    },
  },
};
</script>
