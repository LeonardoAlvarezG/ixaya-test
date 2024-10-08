<template>
    <section>
        <h2>Los Más Nuevos</h2>
        <ul>
            <ProductCard v-for="product in products" :key="product" :product="product" @showToast="ShowToastNotification" />
        </ul>
    </section>
    <div id="notification" :class="showToast" >
        <div class="container">
            <div class="icon"><CheckCircle /></div>
            <p class="message" >¡Producto agregado al carrito!</p>
        </div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import ProductCard from './ProductCard.vue';
    import CheckCircle from "@/components/icons/CheckCircle.vue";

    let products = ref([])
    let productList = []
    const showToast = ref("")

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

    function ShowToastNotification() {
        showToast.value = "show"
        setTimeout(() => {
            showToast.value = ""
        }, 5000)
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

    div#notification {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        visibility: hidden;
        min-width: 250px;
        background-color: #333;
        color: #fff;
        text-align: center;
        border-radius: 0.5rem;
        padding: 1rem;
        position: fixed;
        z-index: 1;
        left: 50%;
        top: 90px;
        font-size: 1rem;
        transform: translate(-50%, 0);

        &.show {
            visibility: visible;
            -webkit-animation: fadein 0.5s, fadeout 0.5s 4.5s;
            animation: fadein 0.5s, fadeout 0.5s 4.5s;
        }
        
    }

    @-webkit-keyframes fadein {
        from {top: 0; opacity: 0;} 
        to {top: 90px; opacity: 1;}
    }

    @keyframes fadein {
        from {top: 0; opacity: 0;} 
        to {top: 90px; opacity: 1;}
    }

    @-webkit-keyframes fadeout {
        from {top: 90px; opacity: 1;} 
        to {top: 0; opacity: 0;}
    }

    @keyframes fadeout {
        from {top: 90px; opacity: 1;}
        to {top: 0; opacity: 0;}
    }
</style>