<template>
    <div>
        <div class="relative">
            <i class="table--search--input--icon fas fa-search "></i>

            <input class="form-control" type="text" id="name" v-model="keyword"  />

        </div>
        <table class="table">
            <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Details</th>
                <th scope="col">Actions</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="product in products" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.details }}</td>
                <td>
                    <div class="row gap-3">
                        <router-link :to="`/products/${product.id}`" class="p-2 col border btn btn-primary">View</router-link>
                        <router-link :to="`/products/${product.id}/edit`" class="p-2 col border btn btn-success">Edit</router-link>
                        <button @click="deleteProduct(product.id)" type="button" class="p-2 col border btn btn-danger">Delete</button>
                    </div>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios';


export default {
    data() {
        return {
            products: [],
            keyword :''

        }
    },
    watch: {
        keyword(after, before) {
            this.getResults();
        }
    },
    async created() {
        try {
            const response = await axios.get('/api/products');
            this.products = response.data;

        } catch (error) {
            console.error(error);
        }
    },
    methods: {
        async getResults() {
            try {

                const response = await axios.get('/api/search_products', {params: {keyword: this.keyword}})
                this.products = response.data;

            } catch (error) {
                console.error(error);
            }
        },
        async deleteProduct(id) {
            try {
                await axios.delete(`/api/products/${id}`);
                this.products = this.products.filter(product => product.id !== id);
            } catch (error) {
                console.error(error);
            }
        }
    }
}
</script>
