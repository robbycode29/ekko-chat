<template>
    <div class="min-h-screen">
        <div v-if="!searchPressed" class="h-screen flex flex-col bg-blue-900">
            <div class="flex flex-col items-center justify-center gap-3 -translate-y-24 h-screen">
                <h1 class="text-4xl text-white mt-20">EKKO</h1>
                <span class="text-white text-sm">Your Amazon Canada <span
                    class="text-xl">🍁</span> shopping assistant.</span>
                <div class="flex flex-row gap-2 w-full justify-center">
                    <input v-model="search" type="text" class="w-1/2 p-2 rounded-lg" placeholder="Search or chat about products...">
                    <button v-if="search" @click="searchProduct" class="bg-blue-500 text-white p-2 rounded-lg">Search</button>
                </div>
            </div>
        </div>
        <div v-else>
            <div class="flex flex-col items-center gap-3 h-[110px] bg-blue-900">
                <div class="flex flex-row items-center justify-center w-screen mt-8 pl-12">
                    <h1 class="text-4xl text-white">EKKO</h1>
                    <span class="text-2xl">🍁</span>
                    <div class="flex flex-row gap-2 w-full justify-start ml-6">
                        <input v-model="search" type="text" class="w-1/2 p-2 rounded-lg" placeholder="Search or chat about products...">
                        <button v-if="search" @click="searchProduct" class="bg-blue-500 text-white p-2 rounded-lg">Search</button>
                    </div>
                </div>
            </div>
            <div class="flex flex-col rounded-2xl shadow-2xl m-5 p-3 border border-blue-100">
                <p v-if="natLangResponse" class="text-black text-start my-4">{{ natLangResponse }}</p>
                <p v-else class="text-black text-2xl font-bold text-center my-4 justify-self-center self-center">...</p>
            </div>
            <ProductList :products="products"/>
        </div>
    </div>
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import ProductList from '@/components/ProductList.vue'

export default defineComponent({
    name: 'HomeView',
    components: {
        ProductList
    },
    data() {
        return {
            search: '',
            searchPressed: false,
            natLangResponse: '',
            products: [],
        }
    },
    methods: {
        async fetchProducts() {
            try {
                // const response = await fetch(`http://localhost:8000/api/v1/products/search/`, {
                const response = await fetch(`https://ekko-d2a6a6e8b0e7.herokuapp.com/api/v1/products/search/`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        query: this.search,
                        limit: 20,
                    })
                })
                const data = await response.json()
                this.natLangResponse = data.openai_response
                this.products = data.products
            } catch (error) {
                console.error(error)
            }
        },
        searchProduct() {
            this.searchPressed = true
            this.natLangResponse = ''
            this.fetchProducts()
        }
    }
})
</script>
