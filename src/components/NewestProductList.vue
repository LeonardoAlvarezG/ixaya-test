<template>
    <section>
        <h2>Los Más Nuevos</h2>
        <ul>
            <ProductCard v-for="product in products" :key="product" :product="product" />
        </ul>
    </section>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import ProductCard from './ProductCard.vue';

    let products = ref([])
    let productList = []

    onMounted(() => {
        axios.get(
            "https://sandbox.ixaya.net/api/products", {
            headers: {
                'X-API-KEY': '8g84woskwokcwk84c440wkkcowg48ww84o88s8cg'
            }}
        )
        .then((res) => {
            productList = res.data.response
            
            OrderProductList()
            
            ShowProducts()
        })
        .catch((err) => {console.error(err)})
    })

    function ShowProducts() {
        let productsNew = []
    
        productList.forEach((product) => {
            if ( productsNew.length < 5 ) {
                productsNew.push(product)
            }
        })
    
        products.value = productsNew
    }

    function OrderProductList() {
        return productList.sort((a,b) => {
            const date_a = new Date(a.last_update)
            const date_b = new Date(b.last_update)
            
            return date_b.valueOf() - date_a.valueOf()
        })
    }
</script>

<style scoped>
    section {
        display: flex;
        position: relative;
        flex-direction: column;
        flex-grow: 1;
        min-height: 50vh;
        padding: 1rem 2rem;
        background-color: var(--color-background);
        transition: background-color 0.5s;

        &>h2 {
            font-size: 1.75rem;
        }

        &>ul {
            display: flex;
            position: relative;
            flex-direction: row;
            flex-grow: 1;
            justify-content: space-between;
            margin: 1rem 0;
            padding: 0;
            gap: 0.5rem;
        }
    }
</style>