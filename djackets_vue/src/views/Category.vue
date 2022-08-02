<template>
    <div class="page-category"></div>
        <div class="columns is-multiline">
            <div class="coumn is-12">
                <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
            </div>
        </div>

</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
export default {
    name: 'Category',
    data() {
        return {
            category: {
                products: []
            }
        }
    },
    mounted () {
        this.getCategory()
    },
    methods: {
       async getCategory() {
        const categorySlug = this.$route.params.category_slug

        this.$store.commit('setIsLoading', true)

            await axios
                .get(`api/v1/products/${categorySlug}/`)
                .then(response => {
                    this.category = response.data

                    document.title = this.category.name + ' | Djackets'
                })
                .catch(error => {
                    console.log(error)

                     toast ({
                         message: 'Something went wrong. Please try again',
                         type: 'is-danger',
                         dismissible: true,
                         pauseOnHover: true,
                         duration: 2000,
                         position: 'bottom-right',
                         })
                })
            this.$store.commit('setIsLoading', false)
       }
    }
}
</script>