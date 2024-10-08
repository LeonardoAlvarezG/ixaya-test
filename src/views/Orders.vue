<template>
    <section class="section">
        <aside>
            <nav>
                <span><button @click="ShowSection(true)" >Carrito de Compras</button></span>
                <span><button @click="ShowSection(false)" >Mis Pedidos</button></span>
            </nav>
        </aside>
        <div>
            <section v-if="sectionDisplay" >
                <h1>Carrito de Compras</h1>
                <OrderPreview :order="shoppingCart" :bought="false" @saveShoppingCart="SaveShoppingCart" @showModal="ShowToastNotification" @updateProductQty="ChangeProductQty" @deleteProduct="DeleteProduct" />
            </section>
            <section v-else >
                <h1>Mis Pedidos</h1>
                <ul v-if="orders.length">
                    <OrderPreview v-for="order in orders" :key="order" :order="order" :bought="true" />
                </ul>
                <h2 v-else>¡Comienza una historia llena de productos increíbles!</h2>
            </section>
        </div>
    </section>
    <div id="notification" :class="showToast" >
        <div v-if="notificationModal.status === 1" class="container">
            <div class="icon"><CheckCircle /></div>
            <p class="message" >¡Gracias por tu compra!</p>
        </div>
        <div v-else class="container">
            <div class="icon"><Error /></div>
            <p class="message" >Lo sentimos, no pudimos procesar tu compra</p>
        </div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import OrderPreview from '../components/OrderPreview.vue'
    import CheckCircle from "@/components/icons/CheckCircle.vue";
    import Error from "@/components/icons/Error.vue";

    let orderList = []
    const orders = ref([])
    const sectionDisplay = ref(true)
    const shoppingCart = ref(JSON.parse(localStorage.getItem('shopping_cart_products')))
    const notificationModal = ref({})
    const showToast = ref("")

    onMounted(async() => {
        await GetOrders()

    })

    async function GetOrders() {
        axios.get(
            "https://sandbox.ixaya.net/api/orders", {
            headers: {
                'X-API-KEY': '8g84woskwokcwk84c440wkkcowg48ww84o88s8cg'
            }}
        )
        .then((res) => {
            orderList = res.data.response

            orders.value = orderList
        })
        .catch((err) => {console.error(err)})
    }

    function SaveShoppingCart( shopping_cart ) {
        localStorage.setItem("shopping_cart_products", JSON.stringify(shopping_cart))
        shoppingCart.value = JSON.parse(localStorage.getItem('shopping_cart_products'))
    }

    function ShowSection(state) {
        sectionDisplay.value = state
    }

    async function ShowToastNotification( info ) {
        notificationModal.value = info
        showToast.value = "show"
        setTimeout(() => {
            showToast.value = ""
        }, 5000)
        await GetOrders()
    }

    function ChangeProductQty(product_updated) {
        shoppingCart.value.some(product => {
            if (product.id === product_updated.id) {
                product.qty = product_updated.qty
                return
            }
        })
        localStorage.setItem("shopping_cart_products", JSON.stringify(shoppingCart.value))
    }

    function DeleteProduct(product_id) {
        const shoppingUpdated = shoppingCart.value.filter((product) => {
            return product.id !== product_id
        })
        shoppingCart.value = shoppingUpdated
        localStorage.setItem("shopping_cart_products", JSON.stringify(shoppingCart.value))
    }
</script>

<style scoped>
    section.section {
        position: relative;
        display: flex;
        flex-grow: 1;

        &>aside {
            position: fixed;
            display: flex;
            flex-direction: column;
            height: 100%;

            &>nav {
                display: flex;
                flex-direction: column;
                flex-grow: 1;
                padding: 2rem 0 2rem 2rem;
                min-width: 254px;
                width: 254px;
                font-size: 1rem;

                & button {
                    display: flex;
                    flex-direction: row;
                    align-items: center;
                    justify-content: start;
                    width: 100%;
                    height: 36px;
                    border: 0;
                    border-radius: 0.125rem;
                    color: var(--color-text);
                    background-color: transparent;
                    font-size: 1rem;

                    &:hover {
                        background-color: var(--color-background-shadow);
                    }
                }
            }
        }

        &>div {
            display: flex;
            flex-direction: column;
            flex-grow: 1;
            padding: 2rem;
            margin: 0 0 0 254px;

            &>section {
                display: flex;
                flex-direction: column;
                flex-grow: 1;

                &>h1 {
                    font-size: 2rem;
                    color: var(--color-heading);
                    font-weight: bold;
                    transition: color 0.5s;
                    padding: 0 0 1rem;
                }

                &>ul {
                    display: flex;
                    flex-direction: column;
                    flex-grow: 1;
                    padding: 0;
                    gap: 1rem;
                }
            }
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