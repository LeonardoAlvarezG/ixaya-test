<template>
    <section class="section">
        <aside>
            <nav>
                <span><a href="#shopping_cart" >Carrito de Compras</a></span>
                <span><a href="#orders" >Mis Pedidos</a></span>
            </nav>
        </aside>
        <div>
            <section id="shopping_cart" >
                <h1>Carrito de Compras</h1>
                <OrderPreview :bought="false" />
                <div>
                    <span>Resumen de compra</span>
                    <span>Total</span>
                    <button>Comprar</button>
                </div>
            </section>
            <hr>
            <section id="orders" >
                <h1>Mis Pedidos</h1>
                <ul v-if="orders.length">
                    <OrderPreview v-for="order in orders" :key="order" :order="order" :bought="true" />
                </ul>
                <h2 v-else>¡Comienza una historia llena de productos increíbles!</h2>
            </section>
        </div>
    </section>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import OrderPreview from '../components/OrderPreview.vue'

    let orderList = []
    let orders = ref([])

    onMounted(() => {
        axios.get(
            "https://sandbox.ixaya.net/api/orders", {
            headers: {
                'X-API-KEY': '8g84woskwokcwk84c440wkkcowg48ww84o88s8cg'
            }}
        )
        .then((res) => {
            orderList = res.data.response
            orderList = [
                {
                    "street_name": "Nombre de calle",
                    "zip_code": 37000,
                    "address": "Colonia",
                    "phone": 477000000,
                    "state": "Guanajuato",
                    "city": "León",
                    "product_list": [
                        {
                            "product_id": 1,
                            "qty": 2
                        },
                        {
                            "product_id": 2,
                            "qty": 1
                        }
                    ]
                },
                {
                    "street_name": "Nombre de calle 2",
                    "zip_code": 37000,
                    "address": "Colonia",
                    "phone": 477000000,
                    "state": "Guanajuato",
                    "city": "León",
                    "product_list": [
                        {
                            "product_id": 1,
                            "qty": 2
                        },
                        {
                            "product_id": 2,
                            "qty": 1
                        }
                    ]
                }
            ]

            orders.value = orderList
        })
        .catch((err) => {console.error(err)})
    })
</script>

<style scoped>
    section.section {
        display: flex;
        flex-grow: 1;
        padding: 1rem 2rem;

        &>aside {
            display: flex;
            flex-direction: column;

            &>nav {
                display: flex;
                flex-direction: column;
                flex-grow: 1;
                padding: 2rem;
                min-width: 254px;
                width: 254px;
                font-size: 1rem;

                & a {
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

            &>section {
                &>h1 {
                    font-size: 2rem;
                    color: var(--color-heading);
                    font-weight: bold;
                    transition: color 0.5s;
                }
            }
        }
    }
</style>