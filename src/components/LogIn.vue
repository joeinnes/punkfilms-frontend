<template>
  <div>
  <form class='form-inline' v-if='loggedInUser'>
    <span class='mr-sm-2'><user-icon style="height: 1em" /> {{loggedInUser}}</span>
    <button class='btn btn-outline-success my-2 my-sm-0' type='submit' v-on:click.prevent='logOut'>Log Out</button>
  </form>

  <form class='form-inline' v-else>
    <input class='form-control mr-2' type='text' placeholder='example@example.com' aria-label='Username' v-model='formUser' required>
    <input class='form-control mr-2' type='password' placeholder='password' aria-label='Password' v-model='formPass' required>
    <button class='btn btn-outline-success my-2 my-sm-0 mr-1' type='submit' v-on:click.prevent='logIn'>Log In</button>
    <router-link to='/register' class='btn btn-outline-success my-2 my-sm-0'>Register</router-link>
  </form>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { UserIcon } from 'vue-feather-icons'
import axios from 'axios'

export default {
  name: 'LogIn',
  data () {
    return {
      formUser: '',
      formPass: ''
    }
  },
  computed: {
    ...mapGetters({
      loggedInUser: 'getUsername'
    })
  },
  methods: {
    logIn: function () {
      axios.post(`${this.$config.API}/auth/local`, {
        identifier: this.formUser,
        password: this.formPass
      })
        .then(response => {
          if (!response.data.jwt) {
            throw response
          }
          this.$store.commit('setUserAndToken', {
            username: response.data.user.username,
            token: response.data.jwt
          })
        })
        .catch(err => {
          console.error(`Caught an error: ${JSON.stringify(err)}`)
          if (err.message) alert(err.message)
        })
      this.formUser = ''
      this.formPass = ''
    },
    logOut: function () {
      this.$store.commit('logOut')
      this.$router.push('/')
    }
  },
  components: {
    UserIcon
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>

</style>
