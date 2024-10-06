<template>
    <!-- <div>
        <h2>{{ product?.title }}</h2>
        <textarea>{{ product?.description }}</textarea>
    </div> -->
    <li>
        <div class="card">
            <button class="addShoppingCart" >
                <AddShoppingCart />
            </button>
            <img :src=product?.image_url :alt=product?.title loading="lazy" decoding="async" >
            <div class="container">
                <h3>{{ product?.title }}</h3>
                <p class="price">${{ productPrice }}</p>
                <h4>${{ productSalePrice }}</h4>
                <p  class="description" :show="isDetailsShown">{{ product?.description }}</p>
            </div>
            <button @click="toggleDetails">Detalles</button>
        </div>
    </li>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import AddShoppingCart from './icons/AddShoppingCart.vue';

    let productPrice = ref(null)
    let productSalePrice = ref(null)
    let isDetailsShown = ref(false)

    onMounted(() => {
        productPrice.value = Number(props.product?.price).toLocaleString('es-MX')
        productSalePrice.value = (Number(props.product?.price) - Number(props.product?.discount)).toLocaleString('es-MX')
    })

    const props = defineProps({
        product: Object
    })

    function toggleDetails() {
        isDetailsShown.value = !isDetailsShown.value
    }
</script>

<style scoped>
    li {
        display: flex;
        flex-direction: column;
        min-width: 249px;
        max-width: 300px;
        width: 100%;
        list-style: none;

        &>div.card {
            display: flex;
            position: relative;
            flex-direction: column;
            max-height: 600px;
            width: 100%;
            height: 100%;
            border-radius: 0.5rem;
            background-color: var(--color-background-mute);
            transition: background-color 0.5s;
            
            &>img {
                width: 100%;
                padding: 1rem;
                object-fit: contain;
                aspect-ratio: 1;
            }
            
            &>div.container {
                display: flex;
                position: relative;
                flex-direction: column;
                flex-grow: 1;
                width: 100%;
                padding: 1rem;
                align-items: center;
                justify-content: space-around;

                &>h3 {
                    display: flex;
                    flex-grow: 1;
                    font-size: 1.125rem;
                }

                &>p {
                    display: flex;
                    align-self: flex-start;
                    font-size: 0.75rem;
                    white-space: pre-wrap;
                    
                    &.price {
                        text-decoration: line-through;
                        opacity: 0.75;
                    }

                    &.description {
                        display: flex;
                        position: absolute;
                        bottom: 0;
                        left: 0;
                        width: 100%;
                        padding: 0.5rem;
                        border-radius: 0.5rem;
                        background-color: var(--color-background-shadow);
                        transition: display 0.2s allow-discrete, opacity 0.2s, scale 0.2s;

                        &[show="true"] {
                            opacity: 1;
                            scale: 1;
                            display: flex;
                        }

                        &[show="false"] {
                            opacity: 0;
                            scale: 0;
                            display: none;
                        }
                    }
                }

                &>h4 {
                    font-size: 1.125rem;
                    font-weight: bold;
                    color: var(--accent-color);
                }
            }

            &>button {
                display: flex;
                position: relative;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                height: 60px;
                width: 100%;
                cursor: pointer;
                margin: 0;
                padding: 0;
                background-color: var(--accent-color);
                color: var(--color-text);
                font-size: 1.125rem;
                border: 0;
                border-bottom-left-radius: 0.5rem;
                border-bottom-right-radius: 0.5rem;

                &:hover {
                    color: var(--color-heading);
                }

                &.addShoppingCart {
                    position: absolute;
                    right: 0;
                    top: 0;
                    width: 50px;
                    height: 50px;
                    background-color: var(--color-background-soft);
                    border: none;
                    border-radius: 50%;
                    opacity: 0.65;
                    transition: opacity 0.2s;

                    &:hover {
                        opacity: 1;
                    }
                }
            }
        }
    }
</style>