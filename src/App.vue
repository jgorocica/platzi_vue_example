<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 {{ msg }}
    select(v-model="selectedCountry")
      option(v-for="(country, index) in countries" :key="index" :value="country.value") {{country.name}}
    
    ul(v-show="!loading")
      Artist(v-for="artist in artists" :key="artist.mbid" :artist="artist") 

    Spinner(v-show="loading")
   
</template>

<script>
import getArtists from './api/service.js'
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'


export default {
  name: 'app',
  components: {
    Artist, 
    Spinner
  }, 
  data () {
    return {
      msg: 'TOP ARTIST MADE WITH Vue and 💚 ', 
      loading: true, 
      selectedCountry: 'mexico', 
      artists: [], 
      countries: [
        {name: 'México', value: 'mexico'},
        {name: 'Venezuela', value: 'venezuela'},
        {name: 'Argentina', value: 'argentina'},
        {name: 'Belice', value: 'belize'}
      ]
    }
  }, 
  methods: {
    getArtists: function() {
      const self = this; 
      this.loading = true; 
      getArtists(this.selectedCountry)
      .then(function( artists ) {
          self.loading = false; 
          self.artists = artists; 
      });
    }
  }, 
  mounted: function () {
     this.getArtists(); 
  }, 
  watch: {
    selectedCountry: function() {
      this.getArtists(); 
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
