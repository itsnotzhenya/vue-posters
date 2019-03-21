<template>
  <div id="app">
    <div class="search">
      <div class="conteiner">
        <form v-on:submit.prevent.enter="poster()">
          <input
            id="name"
            v-model="filmName"
            type="text"
            placeholder="Enter film's name"
            style="display: inline-block"
          >
          <input type="button" value="Search" @click="poster()">
        </form>
        <!-- {{cinema.Search.Title}} -->
      </div>
    </div>
    <div class="content">
      <div id="images" v-for="poster in cinema" :key="poster.id">
        <img :src="cinema.Search.Poster">
        <p>{{cinema.Search.Title}}</p>
        <p>{{cinema.Search.Year}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      // posters: "",
      Search: [],
      filmName: "",
      url: "http://www.omdbapi.com/?apikey=2d10a7d&s=",
      cinema: "",
      title: ""
      // data: ""
    };
  },
  methods: {
    poster() {
      this.getPoster(this.filmName)
        .then(result => {
          // console.log(result);
          this.cinema = JSON.parse(result);
          // this.cinema = result;
          console.log(this.cinema);
          console.log(this.cinema.Search[1].Title);
        })
        .catch(e => {
          alert("Ничего не найдено" + e);
        });
    },
    getPoster(filmName) {
      const url = this.url + filmName;
      return new Promise(function(resolve, reject) {
        let request = new XMLHttpRequest();
        console.log(url);
        request.open("GET", url, true);

        request.onload = function() {
          if (this.status == 200) {
            resolve(this.response);
            this.data = JSON.parse(this.response);
            // console.log(this.data.Search);
            return this.data.Search;
          } else {
            let error = new Error(this.statusText);
            error.code = this.status;
            reject(error);
          }
        };

        request.onerror = function() {
          reject(new Error("Network Error"));
        };

        request.send();
      });
    }
  }
};
</script>

<style>
</style>
