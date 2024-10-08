<template>
    <div v-if="bought" class="section orders" >
        <section class="products" >
            <OrderProductPreview v-for="product in products" :key="product" :product="product" :bought="bought" />
        </section>
        <section class="aside" >
            <span>Detalle de compra</span>
            <div class="products" >
                <span>Productos</span>
                <span>{{ productsQty }}</span> 
            </div>
            <div class="discount" >
                <span>Descuento</span>
                <span>{{ productsDiscount }}</span>
            </div>
            <div class="total" >
                <span>Total</span>
                <span>{{ productsTotal }}</span>
            </div>
            <div class="details" >
                <div>
                    <label for="state">Estado:</label>
                    <span id="state" >{{ order?.state }}</span>
                </div>
                <div>
                    <label for="city">Ciudad:</label>
                    <span id="city" >{{ order?.city }}</span>
                </div>
                <div>
                    <label for="address">Colonia:</label>
                    <span id="address" >{{ order?.address }}</span>
                </div>
                <div>
                    <label for="street_name">Calle:</label>
                    <span id="street_name" >{{ order?.street_name }}</span>
                </div>
                <div>
                    <label for="zip_code">Código Postal:</label>
                    <span id="zip_code" >{{ order?.zip_code }}</span>
                </div>
                <div>
                    <label for="phone">Teléfono:</label>
                    <span id="phone" >{{ phoneFormat }}</span>
                </div>
            </div>
        </section>
    </div>
    <div v-else class="section shopping_cart" >
        <section class="products" >
            <OrderProductPreview v-for="product in products" :key="product" :product="product" :bought="bought" @changeQty="ChangeProductQty" @deleteProduct="DeleteProduct" />
        </section>
        <section class="aside" >
            <span>Resumen de compra</span>
            <div class="products" >
                <span>Productos</span>
                <span>{{ productsQty }}</span> 
            </div>
            <div class="discount" >
                <span>Descuento</span>
                <span>{{ productsDiscount }}</span>
            </div>
            <div class="total" >
                <span>Total</span>
                <span>{{ productsTotal }}</span>
            </div>
            <form class="address" @submit.prevent="MakeOrder" >
                <button type="submit">Comprar</button>
                <div>
                    <label for="state">Estado</label>
                    <input type="text" name="state" id="state" v-model="state" required autocomplete="address-level1" />
                    <span v-if="errors.state">{{ errors.state }}</span>
                </div>
                <div>
                    <label for="city">Ciudad</label>
                    <input type="text" name="city" id="city" v-model="city" required autocomplete="address-level2" />
                    <span v-if="errors.city">{{ errors.city }}</span>
                </div>
                <div>
                    <label for="address">Colonia</label>
                    <input type="text" name="address" id="address" v-model="address" required autocomplete="address-line3" />
                    <span v-if="errors.address">{{ errors.address }}</span>
                </div>
                <div>
                    <label for="street_name">Calle</label>
                    <input type="text" name="street_name" id="street_name" v-model="street_name" required autocomplete="address-line1" />
                    <span v-if="errors.street_name">{{ errors.street_name }}</span>
                </div>
                <div>
                    <label for="zip_code">Código postal</label>
                    <input type="text" name="zip_code" id="zip_code" v-model="zip_code" pattern="\d{5}" required placeholder="12345" autocomplete="postal-code" />
                    <span v-if="errors.zip_code">{{ errors.zip_code }}</span>
                </div>
                <div>
                    <label for="phone">Teléfono</label>
                    <input type="tel" name="phone" id="phone" v-model="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" required placeholder="123-456-7890" autocomplete="tel" />
                    <span v-if="errors.phone">{{ errors.phone }}</span>
                </div>
            </form>
        </section>
    </div>
</template>

<script setup>
    import { ref, onMounted, watch } from 'vue'
    import axios from 'axios';
    import OrderProductPreview from './OrderProductPreview.vue';

    const products = ref([])
    const productsQty = ref(0)
    const productsDiscount = ref(0)
    const productsTotal = ref(0)
    const state = ref('')
    const city = ref('')
    const address = ref('')
    const street_name = ref('')
    const zip_code = ref('')
    const phone = ref('')
    const errors = ref({
        state: null,
        city: null,
        address:null,
        street_name: null,
        zip_code: null,
        phone: null
    })
    const phoneFormat = ref('')

    onMounted(() => {
        if (props.bought) {
            products.value = props.order?.products
            refreshDetails('', '', 'false')
            phoneFormat.value = props.order?.phone.replace(/(\d{3})(\d{3})(\d{4})/, "$1-$2-$3")
        } else {
            products.value = props.order
            refreshDetails('', '', 'false')
        }
        watch(props.order, refreshDetails)
    })

    const props = defineProps({
        order: Object,
        bought: Boolean
    })

    const emits = defineEmits(['saveShoppingCart','showModal','updateProductQty','deleteProduct'])

    function refreshDetails(newValue, oldValue, clear) {
        if (!props.bought) {
            if (clear === "clear") {
                emits('saveShoppingCart', [])
                products.value = []
            } else {
                emits('saveShoppingCart', products.value)
            }
        }

        productsQty.value = 0
        productsDiscount.value = 0
        productsTotal.value = 0
        
        products.value.forEach(product => {
            productsQty.value += Number(product.qty)
            productsDiscount.value += Number(product.qty) * Number(product.discount)
            productsTotal.value += (Number(product.price) - Number(product.discount)) * Number(product.qty)
        });

        productsDiscount.value = '$' + productsDiscount.value.toLocaleString('es-MX')
        productsTotal.value = '$' + productsTotal.value.toLocaleString('es-MX')
    }

    async function MakeOrder() {
        // Validación de campos no vacíos
        errors.value.state = state.value ? null : "Ingrese su estado"
        errors.value.city = city.value ? null : "Ingrese su ciudad"
        errors.value.address = address.value ? null : "Ingrese su colonia"
        errors.value.street_name = street_name.value ? null : "Ingrese su calle"
        errors.value.zip_code = zip_code.value ? null : "Ingrese su código postal"
        errors.value.phone = phone.value ? null : "Ingrese su teléfono"

        // Validación de formato
        const patternZipCode = /\d{5}/;
        const patternPhone = /[0-9]{3}-[0-9]{3}-[0-9]{4}/;
        errors.value.zip_code = patternZipCode.test ? null : "Código inválido. Formato: 12345"
        errors.value.phone = patternPhone.test ? null : "Teléfono inválido. Formato: 123-456-7890"

        // Recolección de datos
        const telephone = Number(phone.value.replace(/-/g, ""))
        const new_order = {
            "street_name": street_name.value,
            "zip_code": Number(zip_code.value),
            "address": address.value,
            "phone": telephone,
            "state": state.value,
            "city": city.value,
            "product_list": []
        }

        products.value.forEach(product => {
            new_order.product_list.push({
                "product_id": Number(product.id),
                "qty": Number(product.qty)
            })
        })

        // Solicitud de creación de orden
        axios.post(
            "https://sandbox.ixaya.net/api/orders/create", new_order, {
                headers: {
                'X-API-KEY': '8g84woskwokcwk84c440wkkcowg48ww84o88s8cg'
                }
            }
        )
        .then((res) => {
            emits('showModal',res.data)
            refreshDetails('','',"clear")
        })
        .catch((err) => {
            console.error(err)
            emits('showModal',err)
        })
    }

    function ChangeProductQty(product_updated) {
        emits('updateProductQty',product_updated)
    }

    function DeleteProduct(product_id) {
        const shoppingUpdated = products.value.filter((product) => {
            return product.id !== product_id
        })
        products.value = shoppingUpdated
        emits('deleteProduct',product_id)
        refreshDetails('','','')
    }
</script>

<style scoped>
    div.section {
        display: flex;
        flex-direction: row;
        flex-grow: 1;
        background-color: var(--color-background-soft);
        border: 0;
        border-radius: 0.5rem;
        transition: background-color 0.5s;

        &>section {
            display: flex;
            flex-direction: column;
            padding: 1rem;

            &.products {
                flex-grow: 1;
                gap: 1rem;
            }

            &.aside {
                min-width: 254px;
                width: 254px;
                border: 0;
                border-left: solid var(--color-border);
                transition: border-left 0.5s;

                &>span {
                    margin: 0 0 1rem;
                }

                &>div {
                    display: flex;
                    flex-direction: row;
                    align-items: center;
                    justify-content: space-between;

                    &.total {
                        margin: 1rem 0;
                    }

                    &.details {
                        display: flex;
                        flex-direction: column;
                        flex-grow: 1;
                        margin: 1rem 0 0;
                        gap: 1rem;
                        justify-content: start;

                        &>div {
                            display: flex;
                            flex-direction: row;
                            align-items: center;
                            justify-content: space-between;
                            width: 100%;
                            gap: 1rem;

                            &>span {
                                flex-grow: 1;
                                text-align: end;
                            }
                        }
                    }
                }

                &>form.address {
                    display: flex;
                    flex-direction: column;
                    flex-grow: 1;
                    justify-content: start;
                    margin: 1rem 0;
                    gap: 1rem;

                    &>button {
                        display: flex;
                        flex-direction: row;
                        align-items: center;
                        justify-content: center;
                        height: 36px;
                        border: 0;
                        border-radius: 1.125rem;
                        color: var(--color-text);
                        background-color: var(--accent-color);
                        font-size: 1rem;
                        cursor: pointer;
                    }

                    &>div {
                        display: flex;
                        flex-direction: column;
                        justify-content: left;

                        &>label {
                            width: 100%;
                        }

                        &>input {
                            width: 100%;
                            height: 36px;
                            border-radius: 0.5rem;
                            background-color: var(--color-background);
                            caret-color: var(--color-text);
                            font-size: 1rem;
                            color: var(--color-text);
                        }
                    }
                }
            }
        }
    }
</style>