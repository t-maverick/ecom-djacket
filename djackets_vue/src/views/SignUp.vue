<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title has-text-centered">Sign up</h1>

                <form @submit.prevent="submitForm">

                  <!--username-->
                  <div class="field mt-5 control has-icons-left has-icons-right">
                    <input type="text" class="input" placeholder="Username" v-model="username">
                    <span class="icon is-small is-left">
                      <i class="fas fa-user"></i>
                    </span>
                    <span class="icon is-small is-right">
                      <i class="fas fa-check"></i>
                    </span>
                  </div>

                  <!--Email-->
                  <div class="field mt-5">
                    <p class="control has-icons-left has-icons-right">
                      <input class="input" type="email" placeholder="Email" v-model="email">
                      <span class="icon is-small is-left">
                        <i class="fas fa-envelope"></i>
                      </span>
                      <span class="icon is-small is-right">
                        <i class="fas fa-check"></i>
                      </span>
                    </p>
                  </div>

                  <!--password1-->
                  <div class="field mt-5">
                    <p class="control has-icons-left">
                      <input class="input" type="password" placeholder="Password" v-model="password">
                      <span class="icon is-small is-left">
                        <i class="fas fa-lock"></i>
                      </span>
                    </p>
                  </div>

                  <!--password2-->
                  <div class="field mt-5">
                    <p class="control has-icons-left">
                      <input class="input" type="password" placeholder="Confirm Password" v-model="password2">
                      <span class="icon is-small is-left">
                        <i class="fas fa-lock"></i>
                      </span>
                    </p>
                  </div>

                  <!--error-->
                  <div class="notification is-danger" v-if="errors.length">
                    <button class="delete"></button>
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                  </div>

                  <!--terms-conditions-->
                  <div class="field mt-5">
                    <div class="control">
                      <label class="checkbox">
                        <input type="checkbox">
                        I agree to the <a href="#">terms and conditions</a>
                      </label>
                    </div>
                  </div>

                  <!--buttons-->
                  <div class="field is-grouped mt-5">
                    <div class="control">
                      <button class="button is-dark">Submit</button>
                    </div>
                  </div>

                  <div class="mt-5">
                    Or <router-link to="/log-in">click here</router-link> to log in
                  </div>
                  

                </form>

            </div>
        </div>  

    </div>
</template>

<script>
import axios from 'axios'

import { toast } from 'bulma-toast'

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            email: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    mounted() {
        document.title = 'Sign up | Dajckets'
    },
    methods: {
      submitForm() {
        this.errors = []

        if (this.username === '') {
            this.errors.push('The username is missing')
        }

        if (this.email === '') {
            this.errors.push('The email is missing')
        }

        if (this.password === '') {
            this.errors.push('The password is missing')
        }

        if (this.password !== this.password2) {
            this.errors.push('The password doesn\'t match')
        }

        if (!this.errors.length) {
            const formData = {
                username: this.username,
                email: this.email,
                password: this.password
            }

            axios 
                .post("/api/v1/users/", formData)
                .then(response => {
                  toast({
                    message: 'Account created, please log in!',
                    type: 'is-success',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-right',
                  })

                  this.$router.push('/log-in')
                })
                .catch(error => {
                  if (error.response) {
                    for (const property in error.response.data) {
                      this.errors.push(`${property}: ${error.response.data[property]}`)
                    }

                    console.log(JSON.stringify(error.response.data))
                  } else if (error.message) {
                      this.errors.push('Something went wrong. Please try again')

                      console.log(JSON.stringify(error))
                  }
                })
        }
      }
    }

}

</script>