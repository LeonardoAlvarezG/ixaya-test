<template>
    <section class="section">
        <nav>
            <span><button @click="SetDepartment('all', $event)" >Todos los departamentos</button></span>
            <ul>
                <li v-for="department in departments" :key="department">
                    <button @click="SetDepartment(department, $event)" >{{ department }}</button>
                </li>
            </ul>
        </nav>
        <section>
            <teleport to='body' >
                <dialog ref="dialogProduct" class="product" >
                    <button @click="HideModal" class="close" ><Close /></button>
                    <img :src=productModal?.image_url :alt=productModal?.title loading="lazy" decoding="async" >
                    <div class="container">
                        <h3>{{ productModal?.title }}</h3>
                        <p class="price">${{ productPrice }}</p>
                        <div>
                            <p class="discount" >{{ productDiscount }}</p>
                            <h4>${{ productSalePrice }}</h4>
                        </div>
                        <p  class="description" >{{ productModal?.description }}</p>
                    </div>
                </dialog>
            </teleport>
            <h1>Productos</h1>
            <h2>{{ departmentSelected }}</h2>
            <ul>
                <ProductCard v-for="product in products" :key="product" :product="product" :large="true" @openModal="ShowModal" @showToast="ShowToastNotification" />
            </ul>
        </section>
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
    import ProductCard from '../components/ProductCard.vue'
    import Close from "@/components/icons/Close.vue";
    import CheckCircle from "@/components/icons/CheckCircle.vue";

    const products = ref([])
    const departments = ref(new Set())
    const searchParams = ref(new URLSearchParams(window.location.search))
    let productList
    const dialogProduct = ref(null)
    const productModal = ref({})
    const productPrice = ref(null)
    const productSalePrice = ref(null)
    const productDiscount = ref(null)
    const departmentSelected = ref("Todos los departamentos")
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
            
            ShowProducts(searchParams.value.get("department"))

            products.value.forEach(product => {
                departments.value.add(product.category)
            });
        })
        .catch((err) => {console.error(err)})
    })

    function ShowProducts(searchParam) {
        if (searchParam === "all" || searchParam === null) {
            products.value = productList
        } else {
            let productsNew = []

            productList.forEach((product) => {
                if (product.category === searchParam) {
                    productsNew.push(product)
                }
            })

            products.value = productsNew
        }
    }

    function SetDepartment(departmentName, event) {
        searchParams.value.set("department", departmentName)

        departmentSelected.value = event.target.innerText

        ShowProducts(searchParams.value.get("department"))
    }

    function ShowModal( product ) {
        productModal.value = product
        productPrice.value = Number(productModal.value?.price).toLocaleString('es-MX')
        productSalePrice.value = (Number(productModal.value?.price) - Number(productModal.value?.discount)).toLocaleString('es-MX')
        productDiscount.value = `-${Math.floor(Number(productModal.value?.discount) * 100 / Number(productModal.value?.price))}%`
        dialogProduct.value.showModal()
    }
    
    function HideModal() {
        dialogProduct.value.close()

    }

    function ShowToastNotification() {
        showToast.value = "show"
        setTimeout(() => {
            showToast.value = ""
        }, 5000)
    }
</script>

<style scoped>
    section.section {
        display: flex;
        flex-direction: row;
        flex-grow: 1;

        &>nav {
            position: fixed;
            display: flex;
            flex-direction: column;
            padding: 2rem 0 2rem 2rem;
            min-width: 254px;
            width: 254px;
            font-size: 1rem;
            height: 100%;
            
            &>ul {
                position: relative;
                display: flex;
                flex-direction: column;
                width: 100%;
                padding: 0 0 0 1.5rem;
                
                &>li {
                    display: flex;
                    /* height: 36px; */
                }
            }
            
            & button {
                display: flex;
                flex-direction: row;
                align-items: center;
                justify-content: start;
                width: 100%;
                min-height: 36px;
                border: 0;
                border-radius: 0.125rem;
                color: var(--color-text);
                background-color: transparent;
                font-size: 1rem;
                text-align: start;

                &:hover {
                    background-color: var(--color-background-shadow);
                }
            }
        }

        &>section {
            display: flex;
            flex-direction: column;
            flex-grow: 1;
            padding: 2rem;
            margin: 0 0 0 254px;

            &>h1 {
                font-size: 2rem;
                color: var(--color-heading);
                font-weight: bold;
                transition: color 0.5s;
            }

            &>h2 {
                font-size: 1.5rem;
            }

            &>ul {
                display: flex;
                justify-content: space-between;
                padding: 1rem 0 0;
                flex-wrap: wrap;
                gap: 1rem;
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