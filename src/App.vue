<template lang="pug">
  #app
    img(src='https://carlrdj.github.io/music-vuejs/dist/logo.png')
    h1 {{ title }}
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue';
import Spinner from './components/Spinner.vue';
import getArtists from './api';
export default {
  name: 'app',
  data () {
    return {
      title: 'Music',
      artists: [],
      countries: [
        {name: 'España', value: 'spain'},
        {name: 'Peru', value: 'peru'}
      ],
      selectedCountry: 'peru',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists(){
      const self = this;
      self.loading = true
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.artists = artists
          self.loading = false
        })
    }
  },
  mounted() {
    this.refreshArtists();
  },
  watch:{
    selectedCountry() {
      this.refreshArtists(this.selectedCountry);
    }
  }
}
</script>

<style lang="stylus">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
