<template lang="pug">
  #app
    img(src='https://clissmancamacho.github.io/platzimusic-vuejs/dist/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for = "artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
  import Artist from './components/Artist.vue'
  import Spinner from './components/Spinner.vue'
  import getArtists from './api'

  export default {
    name: 'app',
    data () {
      return {
        artists: [],
        countries: [
          { name: 'Argentina', value: 'argentina'},
          { name: 'Colombia', value: 'colombia'},
          { name: 'Venezuela', value: 'venezuela'},
          { name: 'España', value: 'spain'},
        ],
        selectedCountry: 'venezuela',
        loading: true
      }
    },
    components: {
      Artist,
      Spinner
    },
    methods: {
      refreshArtist: function () {
        const self = this
        self.loading = true
        self.artists = []
        getArtists(this.selectedCountry)
          .then(function (artists) {
            self.loading = false
            self.artists = artists
          })
      }
    },
    mounted () {
      this.refreshArtist()
    },
    watch: {
      selectedCountry() {
        this.refreshArtist()
      }
    }
  }
</script>

<style lang="stylus">
  #app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

  h1, h2
    font-weight normal

  ul
    list-style-type none
    padding 0

  li
    display inline-block
    margin 0 10px

  a
    color #42b983
  
</style>
