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
                <OrderPreview :order="shoppingCart" :bought="false" />
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
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import OrderPreview from '../components/OrderPreview.vue'

    let orderList = []
    const orders = ref([])
    const sectionDisplay = ref(true)
    const shoppingCart = ref(JSON.parse(localStorage.getItem('shopping_cart_products')))

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
                            "id": "1",
                            "category": "Tecnología",
                            "title": "Apple Laptop MacBook Air de de 2020",
                            "short_description": "Chip M1 de, Pantalla Retina de 13 Pulgadas, 8 GB de RAM, Almacenamiento SSD de 256 GB, Teclado retroiluminado, cámara FaceTime HD y Touch ID - Color Oro",
                            "description": "Acerca de este artículo\nDevoluciones: Sólo productos dañados, inoperantes al primer uso o sin abrir en caja original sellada.\nBatería para todo el día – Hasta 18 horas de batería según el uso, para hacer mucho más.\nRendimiento extraordinario – Para hacer de todo, desde editar con calidad profesional hasta disfrutar los mejores juegos de acción. El chip M1 de Apple con CPU de 8 núcleos ofrece un rendimiento hasta 3.5 veces más rápido que la generación anterior, con menos consumo de batería.\nMemoria ultrarrápida – 8 GB de memoria unificada para mayor velocidad y fluidez de todo el sistema. Ejecuta de manera rápida y fácil tareas que ocupan mucha memoria, como navegar con varias pestañas abiertas al mismo tiempo o abrir un archivo gráfico de gran tamaño.\nPantalla espectacular – Con una pantalla Retina de 13.3 pulgadas, las imágenes cobran vida con un nivel superior de realismo. Los textos se ven increíblemente nítidos y los colores son más vibrantes.",
                            "sale_count": "255",
                            "discount": "1000",
                            "price": "23000",
                            "enabled": "1",
                            "image_url": "https://sandbox.ixaya.net/media/products/71vFKBpKakL._AC_SL1500_.jpg",
                            "create_date": "2020-11-01 00:00:00",
                            "last_update": "2023-05-16 23:08:32",
                            "qty": 2,
                            "total": "44000"
                        },
                        {
                            "id": "2",
                            "category": "Tecnología",
                            "title": "Apple Nuevo MacBook Pro Chip M1",
                            "short_description": "Apple Nuevo MacBook Pro Chip M1 de (de 13 Pulgadas, 8 GB RAM, 256 GB SSD) - Color Plata (Ultimo Modelo)",
                            "description": "Acerca de este artículo\nDevoluciones: Sólo productos dañados, inoperantes al primer uso o sin abrir en caja original sellada.\nChip M1 de Apple que permite un gran avance en el rendimiento del CPU, GPU y aprendizaje automático\nLa mayor duración de batería en una Mac: hasta 20 horas para que puedas hacer mucho más\nCPU de 8 núcleos con un rendimiento hasta 2.8 veces más rápido para ejecutar flujos de trabajo a una velocidad increíble\nGPU de hasta 8 núcleos con gráficos hasta 5 veces más veloces para apps y juegos con gráficos avanzados\nNeural Engine de 16 núcleos para un aprendizaje automático avanzado\n8 GB de memoria unificada para que todo sea más rápido y fluido\nAlmacenamiento SSD superrápido para abrir apps y archivos al instante\nPantalla Retina de 13.3 pulgadas con 500 nits de brillo para que disfrutes imágenes vibrantes y un nivel de detalle increíble\nVisítanos directamente en la página de Apple para agregar la cobertura de AppleCare+",
                            "sale_count": "55",
                            "discount": "499",
                            "price": "34499",
                            "enabled": "1",
                            "image_url": "https://sandbox.ixaya.net/media/products/71gD8WdSlaL._AC_SL1500_.jpg?v=3",
                            "create_date": "2020-11-01 00:00:00",
                            "last_update": "2022-01-11 22:04:45",
                            "qty": 1,
                            "total": "34000"
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
                            "id": "1",
                            "category": "Tecnología",
                            "title": "Apple Laptop MacBook Air de de 2020",
                            "short_description": "Chip M1 de, Pantalla Retina de 13 Pulgadas, 8 GB de RAM, Almacenamiento SSD de 256 GB, Teclado retroiluminado, cámara FaceTime HD y Touch ID - Color Oro",
                            "description": "Acerca de este artículo\nDevoluciones: Sólo productos dañados, inoperantes al primer uso o sin abrir en caja original sellada.\nBatería para todo el día – Hasta 18 horas de batería según el uso, para hacer mucho más.\nRendimiento extraordinario – Para hacer de todo, desde editar con calidad profesional hasta disfrutar los mejores juegos de acción. El chip M1 de Apple con CPU de 8 núcleos ofrece un rendimiento hasta 3.5 veces más rápido que la generación anterior, con menos consumo de batería.\nMemoria ultrarrápida – 8 GB de memoria unificada para mayor velocidad y fluidez de todo el sistema. Ejecuta de manera rápida y fácil tareas que ocupan mucha memoria, como navegar con varias pestañas abiertas al mismo tiempo o abrir un archivo gráfico de gran tamaño.\nPantalla espectacular – Con una pantalla Retina de 13.3 pulgadas, las imágenes cobran vida con un nivel superior de realismo. Los textos se ven increíblemente nítidos y los colores son más vibrantes.",
                            "sale_count": "255",
                            "discount": "1000",
                            "price": "23000",
                            "enabled": "1",
                            "image_url": "https://sandbox.ixaya.net/media/products/71vFKBpKakL._AC_SL1500_.jpg",
                            "create_date": "2020-11-01 00:00:00",
                            "last_update": "2023-05-16 23:08:32",
                            "qty": 2,
                            "total": "44000"
                        },
                        {
                            "id": "2",
                            "category": "Tecnología",
                            "title": "Apple Nuevo MacBook Pro Chip M1",
                            "short_description": "Apple Nuevo MacBook Pro Chip M1 de (de 13 Pulgadas, 8 GB RAM, 256 GB SSD) - Color Plata (Ultimo Modelo)",
                            "description": "Acerca de este artículo\nDevoluciones: Sólo productos dañados, inoperantes al primer uso o sin abrir en caja original sellada.\nChip M1 de Apple que permite un gran avance en el rendimiento del CPU, GPU y aprendizaje automático\nLa mayor duración de batería en una Mac: hasta 20 horas para que puedas hacer mucho más\nCPU de 8 núcleos con un rendimiento hasta 2.8 veces más rápido para ejecutar flujos de trabajo a una velocidad increíble\nGPU de hasta 8 núcleos con gráficos hasta 5 veces más veloces para apps y juegos con gráficos avanzados\nNeural Engine de 16 núcleos para un aprendizaje automático avanzado\n8 GB de memoria unificada para que todo sea más rápido y fluido\nAlmacenamiento SSD superrápido para abrir apps y archivos al instante\nPantalla Retina de 13.3 pulgadas con 500 nits de brillo para que disfrutes imágenes vibrantes y un nivel de detalle increíble\nVisítanos directamente en la página de Apple para agregar la cobertura de AppleCare+",
                            "sale_count": "55",
                            "discount": "499",
                            "price": "34499",
                            "enabled": "1",
                            "image_url": "https://sandbox.ixaya.net/media/products/71gD8WdSlaL._AC_SL1500_.jpg?v=3",
                            "create_date": "2020-11-01 00:00:00",
                            "last_update": "2022-01-11 22:04:45",
                            "qty": 1,
                            "total": "34000"
                        }
                    ]
                }
            ]

            orders.value = orderList
        })
        .catch((err) => {console.error(err)})

    })

    function ShowSection(state) {
        sectionDisplay.value = state
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
</style>