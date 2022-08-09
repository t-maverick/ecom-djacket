<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title has-text-centered">Log in</h1>

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

                  <!--password1-->
                  <div class="field mt-5">
                    <p class="control has-icons-left">
                      <input class="input" type="password" placeholder="Password" v-model="password">
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
                      <button class="button is-dark">Log in</button>
                    </div>
                  </div>

                  <div class="mt-5">
                    Or <router-link to="/sign-up">click here</router-link> to sign up
                  </div>
                  

                </form>

            </div>
        </div>  

    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'LogIn',
    data() {
        return {
            username: '',
            password: '',
            errors: []
        }
    },
    mounted() {
        document.title = 'Log in | Djackets'
    },
    methods: {
        async submitForm() {
            axios.defaults.headers.common['Authorization'] = ""

            localStorage.removeItem('token')

            const formData = {
                username: this.username,
                password: this.password
            }

            await axios
                .post("/api/v1/token/login", formData)
                .then(response => {
                    const token = response.data.auth_token

                    this.$store.commit("setToken", token)

                    axios.defaults.headers.common["Authorization"] = 'Token ' + token

                    localStorage.setItem("token", token)

                    const toPath = this.$route.query.to || '/cart'

                    this.$router.push(toPath)

                })
                .catch(error => {
                    if (error.response) {
                    for (const property in error.response.data) {
                      this.errors.push(`${property}: ${error.response.data[property]}`)
                    }
                  } else if (error.message) {
                      this.errors.push('Something went wrong. Please try again')

                      console.log(JSON.stringify(error))
                  }
                })
        }
    }
}
</script>