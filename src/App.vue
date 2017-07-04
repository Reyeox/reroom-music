<template>
  <div id="app">
  
    <img src="https://reyeox.github.io/reroom-music/dist/Site-Icon.png" width="10%" height="10%">
    <h1></h1>
    <h2>ReRoom Music</h2>
    <select v-model="selectedCountry"><option v-for="contry in countries" v-bind:value="contry.value">{{contry.name}}</option></select>
    <ul>
      <spinner v-show="loading"></spinner>
      <artist v-for="artist in artists" :artist="artist" v-bind:key="artist.mbid"></artist>
    </ul>
  </div>
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner from './components/Spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
      {name: 'Argentina', value:"argentina"},
      {name: 'Venezuela', value:"venezuela"},
      {name: 'Colombia', value:"colombia"},
      ],
      selectedCountry: 'argentina',
      loading: true

    }
  },
  components: {
    Artist,
    Spinner
  },
  methods:{
    refreshArtists(){
    const self = this
    this.loading = true
    this.artists = []
    getArtists(this.selectedCountry)
    .then(function(artists){
      self.loading = false
      self.artists = artists

    })
  }
},
  mounted() {
    this.refreshArtists()
    },
  watch: {
    selectedCountry(){
      this.refreshArtists()
    }
  }
}
</script>

<style>
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
  color: red;
}


</style>
