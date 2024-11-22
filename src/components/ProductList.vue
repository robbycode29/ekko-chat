<template>
    <div class="grid grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4 m-3">
        <a :href="product.product_url" target="_blank" v-for="product in products" :key="product.pinecone_id" class="flex flex-col h-[300px] rounded-2xl shadow-2xl p-3 border gap-2">
            <img :src="product.img_url" alt="product image" class="rounded-lg w-[150px] self-center"/>
            <p class="text-black text-xs text-center">{{ product.title }}</p>
            <p class="text-black text-xs text-center">{{ product.stars }} stars</p>
            <p class="text-black text-xs text-center">{{ product.reviews }} reviews</p>
            <p class="text-black text-xs text-center">{{ product.price }} $</p>
        </a>
        <div v-if="!products.length" class="self-center justify-self-center">
            <p class="text-black text-2xl font-bold text-center my-4 justify-self-center self-center">No products found</p>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'ProductList',
    props: {
        products: {
            type: Array,
            required: true
        }
    },
})
</script>