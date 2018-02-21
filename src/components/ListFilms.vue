<template>
  <div class='mx-4 mx-md-3 mx-lg-2 mx-xl-1'>
    <div class='mt-3 card-columns'>
      <FilmCard v-for='film in films' :key='film.id' :film='film' />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import FilmCard from './FilmCard.vue'

export default {
  name: 'Home',
  components: {
    FilmCard
  },
  data () {
    return {
      films: []
    }
  },
  created () {
    axios.get(`${this.$config.API}/film`, {
      'headers': {
        'Authorization': `Bearer ${this.$store.state.token}`
      }
    })
      .then((response) => {
        this.films = response.data
      })
      .catch((err) => console.error(err))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
