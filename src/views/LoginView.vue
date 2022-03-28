<template>
    <div class="d-flex justify-content-center" style="margin-top: 120px;">
        <div class="col-lg-4 col-md-8 col-sm-12 card">
            <div class="card-header">
                <h3 class="card-title">Login Form</h3>
            </div>
            <form @submit.prevent="submitLogin()">
                <div class="card-body">
                    <div class="mb-3">
                        <label for="input-username" class="form-label">Username</label>
                        <input v-model="login.username" type="text" class="form-control" id="input-username" placeholder="Username">
                    </div>
                    <div class="mb-3">
                        <label for="input-password" class="form-label">Password</label>
                        <input v-model="login.password" type="password" class="form-control" id="input-password" placeholder="name@example.com">
                    </div>
                </div>
                <div class="card-footer">
                    <button type="submit" class="btn btn-primary">Login</button>
                </div>
            </form>
        </div>
    </div>
</template>

<style>
</style>

<script>
import axios from 'axios'

export default {
  name: 'LoginView',
  data () {
    return {
      login: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    submitLogin () {
      const self = this

      axios.post('http://localhost:8000/auth/login', {
        username: this.login.username,
        password: this.login.password
      })
        .then(response => {
          localStorage.setItem('access_token', response.data.token)
          alert('Login Success!')
          self.$router.push('/')
        })
        .catch(function (error) {
          alert(error.message)
        })
    }
  }
}
</script>
