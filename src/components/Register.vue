<template>
  <div class='mt-3'>
    <h1>Register</h1>
    <form>
      <div class="form-group">
        <label for="username">Username</label>
        <input type="text" class="form-control" id="username" aria-describedby="usernameHelp" placeholder="Enter username" v-model='username'>
        <small id="usernameHelp" class="form-text text-muted">Pick a username.</small>
      </div>
      <div class="form-group">
        <label for="email">Email address</label>
        <input type="email" class="form-control" id="email" aria-describedby="emailHelp" placeholder="Enter email" v-model='email'>
        <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
      </div>
      <div class="form-group">
        <label for="password1">Password</label>
        <input type="password" class="form-control" id="password1" placeholder="Password" v-model='pass1'>
        <small id="password1Help" class="form-text text-muted" v-if="!passwordLongEnough">Your password needs to be at least 8 characters!</small>
      </div>
      <div class="form-group">
        <label for="password2">Password again</label>
        <input type="password" class="form-control" id="password2" placeholder="Password"  aria-describedby="password2Help" v-model='pass2'>
        <small id="password2Help" class="form-text text-muted">Confirm your password. {{passwordsMatch ? '' : 'Passwords don\'t match!'}}</small>
      </div>
      <button type="submit" class="btn btn-primary" :disabled="!formValid" @click.prevent="register">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Register',
  data () {
    return {
      username: '',
      email: '',
      pass1: '',
      pass2: ''
    }
  },
  computed: {
    passwordsMatch: function () {
      return this.pass1 === this.pass2
    },
    emailContainsAt: function () {
      return this.email.indexOf('@') > -1
    },
    passwordLongEnough: function () {
      return this.pass1.length >= 8
    },
    formValid: function () {
      return this.passwordsMatch && this.emailContainsAt && this.passwordLongEnough
    }
  },
  methods: {
    register: function () {
      if (!this.formValid) {
        console.error('Form is not valid')
        return
      }
      axios.post(`${this.$config.API}/auth/local/register`, {
        'username': this.username,
        'email': this.email,
        'password': this.pass1
      })
        .then((response) => {
          if (!response.data.jwt) {
            throw response
          }
          this.$store.commit('setUserAndToken', {
            username: response.data.user.username,
            token: response.data.jwt
          })
          this.$router.push('/')
        })
        .catch((err) => console.error(`There was an error registering you: ${err}. This username may not be available, or you may have already registered with this email address.`))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
