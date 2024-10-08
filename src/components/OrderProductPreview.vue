<template>
    <section class="section" >
        <img :src="product?.image_url" :alt="product?.title" loading="lazy" decoding="async" >
        <div class="info" >
            <h3 class="short_description" >{{ product?.short_description }}</h3>
            <div>
                <p v-if="product?.enabled" class="enabled" >Disponible</p>
                <p v-else class="disabled" >No Disponible</p>
                <span class="qty" >
                    Cantidad: 
                    <select name="product_qty" v-model="selected" :disabled="bought" >
                        <option value="1" selected>1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                        <option value="5">5</option>
                    </select>
                </span>
                <button @click="DeleteProduct" class="delete" :disabled="bought" >Eliminar</button>
            </div>
        </div>
        <div class="price">
            <div class="discount">
                <p class="percent">{{ productDiscount }}</p>
                <p class="sale_price">${{ productPrice }}</p>
            </div>
            <h2 class="price" >${{ productSalePrice }}</h2>
        </div>
    </section>
</template>

<script setup>
    import { ref, onMounted, watch } from "vue"

    const productPrice = ref(null)
    const productSalePrice = ref(null)
    const productDiscount = ref(null)
    const selected = ref('')

    onMounted(() => {
        productPrice.value = Number(props.product?.price).toLocaleString('es-MX')
        productSalePrice.value = (Number(props.product?.price) - Number(props.product?.discount)).toLocaleString('es-MX')
        productDiscount.value = `-${Math.floor(Number(props.product?.discount) * 100 / Number(props.product?.price))}%`
        selected.value = props.product?.qty
        
        watch(selected, (newValue) => {
            const productChange = {
                "id": props.product.id,
                "qty": Number(newValue)
            }
            props.product.qty = Number(newValue)
            emits('changeQty', productChange)
        })
    })


    const props = defineProps({
        product: Object,
        bought: Boolean
    })

    const emits = defineEmits(['changeQty', 'deleteProduct'])

    function DeleteProduct() {
        emits('deleteProduct', props.product.id)
    }
</script>

<style scoped>
    section.section {
        display: flex;
        flex-direction: row;
        background-color: var(--color-background-mute);
        padding: 1rem;
        border: 0;
        border-radius: 0.5rem;

        &>img {
            width: 100px;
            object-fit: contain;
            aspect-ratio: 1;
        }

        &>div {
            display: flex;
            flex-direction: column;

            &.info {
                margin: 0 2rem 0 1rem;
                flex-grow: 1;

                &>h3 {
                    display: flex;
                    flex-grow: 1;
                    font-size: 1.25rem;
                    color: var(--color-heading);
                }

                &>div {
                    display: flex;
                    flex-direction: row;
                    align-items: baseline;
                    font-size: 0.75rem;
                    gap: 1rem;
                    height: 26px;
                    
                    &>p.enabled {
                        opacity: 1;
                    }
                    
                    &>p.disabled {
                        opacity: 0.5;
                    }
                    
                    &>span.qty {
                        position: relative;
                        font-size: 1rem;
                        height: 100%;

                        &>select {
                            background-color: var(--color-background);
                            color: var(--text-color);
                            font-size: 1rem;
                            border: 0;
                            border-radius: 0.6rem;
                            padding: 0 0.25rem;
                            width: 40px;
                            height: 100%;
                            cursor: pointer;

                            &:disabled {
                                background-color: transparent;
                                cursor: auto;
                                appearance: none;
                                -webkit-appearance: none;
                                -moz-appearance: none;
                            }
                        }
                    }
                    
                    &>button.delete {
                        height: 100%;
                        border: 0;
                        border-radius: 0.8125rem;
                        padding: 0 0.5rem;
                        background-color: transparent;
                        font-size: 1rem;
                        color: var(--text-color);
                        cursor: pointer;
                        transition: background-color 0.1s;

                        &:hover:not(:disabled) {
                            background-color: var(--color-background);
                        }

                        &:disabled {
                            cursor: auto;
                            opacity: 0.25;
                        }
                    }
                }

            }

            &.price {
                align-items: center;
                justify-content: center;

                &>div.discount {
                    display: flex;
                    flex-direction: row;
                    align-items: center;
                    gap: 0.5rem;

                    &>p.percent {
                        color: rgb(0, 209, 0);
                        font-size: 1rem;
                        line-height: 1;
                    }

                    &>p.sale_price {
                        font-size: 1rem;
                        text-decoration: line-through;
                        opacity: 0.5;
                    }
                }
            }

            &>h2.price {
                font-size: 1.25rem;
                font-weight: bold;
                line-height: 1;
                color: var(--accent-color);
            }
        }
    }
</style>