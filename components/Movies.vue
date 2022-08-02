<template>
  <div>
    <div v-if="isLoading" class="loading">
      <v-progress-circular
        :size="70"
        :width="7"
        color="dark"
        indeterminate
      ></v-progress-circular>
    </div>
    <div v-else>
      <v-container>
        <div class="search">
          <v-text-field
            @keyup.enter="$fetch"
            v-model.lazy="search"
            placeholder="Search Here..."
          >
          </v-text-field>
          <v-btn
            v-if="search !== ''"
            @click="clickSearch"
            style="margin-left: 20px"
            color="error"
            >Clear</v-btn
          >
        </div>
        <div v-if="search === ''">
          <v-row>
            <v-col
              xl="3"
              md="4"
              sm="6"
              xs="12"
              v-for="movie in movies"
              :key="movie.id"
            >
              <v-card>
                <v-img
                  height="250"
                  :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                ></v-img>
                <v-card-title>
                  <h4>
                    {{ movie.title.slice(0, 10)
                    }}{{ movie.title.length > 10 ? "...." : "" }}
                  </h4>
                </v-card-title>
                <v-card-text>
                  <nuxt-link
                    :to="{ name: 'movieid', params: { movieid: movie.id } }"
                  >
                    <v-btn color="primary">Read More</v-btn>
                  </nuxt-link>
                </v-card-text>

                <span id="vote">{{ movie.vote_average }}</span>
              </v-card>
            </v-col>
          </v-row>
        </div>
        <div v-else>
          <v-row>
            <v-col
              xl="3"
              md="4"
              sm="6"
              xs="12"
              v-for="movie in movies"
              :key="movie.id"
            >
              <v-card>
                <v-img
                  height="250"
                  :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                ></v-img>
                <v-card-title>
                  <h4>
                    {{ movie.title.slice(0, 10)
                    }}
                    <!-- {{ movie.title.length > 10 ? "...." : "" }} -->
                  </h4>
                </v-card-title>
                <v-card-text>
                  <nuxt-link
                    :to="{ name: 'movieid', params: { movieid: movie.id } }"
                  >
                    <v-btn color="primary">Read More</v-btn>
                  </nuxt-link>
                </v-card-text>

                <span id="vote">{{ movie.vote_average }}</span>
              </v-card>
            </v-col>
          </v-row>
        </div>
      </v-container>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      movies: [],
      isLoading: false,
      searchMovie: [],
      search: "",
    };
  },
  methods: {
    async getMovies() {
      this.isLoading = true;
      const response = await axios.get(
        "https://api.themoviedb.org/3/movie/now_playing?api_key=48ea694f350f86ea6c318b836a2d7a93&language=en-US&page=1"
      );
      this.movies = response.data.results;
      this.isLoading = false;
    },
    async searchMovies() {
      this.isLoading = true;
      const response = await axios.get(
        `https://api.themoviedb.org/3/search/movie/?api_key=48ea694f350f86ea6c318b836a2d7a93&language=en-US&page=1&query=${this.search}`
      );
      this.searchMovie = response.data.results;
      this.isLoading = false;
    },
    clickSearch() {
      this.search = "";
      this.searchMovie = [];
    },
  },
  async fetch() {
    if (this.search === "") {
      await this.getMovies();
    }
    if (this.search !== "") {
      await this.searchMovies();
    }
  },
};
</script>
<style scoped>
.image {
  position: relative;
}
#vote {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: red;
  padding: 5px;
  border-radius: 10px;
}
.loading {
  display: flex;
  justify-content: center;
  margin: 50px 0px;
}
.search {
  width: 50%;
  margin: 20px 0px;
  display: flex;
  align-items: center;
}
</style>
