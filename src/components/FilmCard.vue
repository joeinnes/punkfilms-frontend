<template>
  <div class='card'>
    <img class='card-img-top img-fluid' :src='film.Image' :alt='film.Title'>
    <div class='card-body'>
      <h5 class="card-title mb-0 pb-0">{{film.Title}}</h5>
      <p class='card-text'>
        <GenreBadge v-for='genre in film.genres' :genre='genre' :key='genre.id'/>
      </p>
      <p class='card-text rating mb-1'>
        <star-icon style='height: 1em' /> {{film.Rating}}/10
      </p>
      <p class='card-text'>
        {{film.Overview}}
      </p>
      <p class='card-text d-flex justify-content-between'>
        <a :href='tmdbLink' target='_blank'>Read more</a>
        <a :href='youtubeSearch' target='_blank'><video-icon /></a>
      </p>
    </div>
    <div class='card-footer d-flex justify-content-between'>
        <thumbs-up-icon /> <thumbs-down-icon /> <eye-icon />
    </div>
  </div>
</template>

<script>
import GenreBadge from './GenreBadge.vue'
import { StarIcon, ThumbsUpIcon, ThumbsDownIcon, EyeIcon, VideoIcon } from 'vue-feather-icons'

export default {
  name: 'FilmCard',
  props: ['film'],
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  computed: {
    tmdbLink () {
      return `https://www.themoviedb.org/movie/${this.film['TMDB ID']}`
    },
    youtubeSearch () {
      return `https://youtube.com/results?search_query=${this.film.Title}%20trailer`
    }
  },
  components: {
    GenreBadge, StarIcon, ThumbsUpIcon, ThumbsDownIcon, EyeIcon, VideoIcon
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
</style>
