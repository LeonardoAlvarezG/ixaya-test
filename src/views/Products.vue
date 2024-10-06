<template>
    <section class="section">
        <nav>
            <span><button @click="SetDepartment('all')">Todos los departamentos</button></span>
            <ul>
                <li v-for="department in departments" :key="department">
                    <button @click="SetDepartment(department)">{{ department }}</button>
                </li>
            </ul>
        </nav>
        <h1>Productos</h1>
        <section>
            <h2>Todos los departamentos</h2>
            <ul id="products">
                <ProductCard v-for="product in products" :key="product" :product="product"/>
            </ul>
        </section>
    </section>
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import axios from "axios"
    import ProductCard from '../components/ProductCard.vue'

    let products = ref([])
    let departments = ref(new Set())
    let searchParams = ref(new URLSearchParams(window.location.search))
    let productList

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

    function SetDepartment(departmentName) {
        searchParams.value.set("department", departmentName)
        searchParams.value.values().forEach(value => {
            console.info(value)
        });

        ShowProducts(searchParams.value.get("department"))
    }
</script>

<style scoped>
    section.section {
        display: flex;
        flex-direction: row;
        flex-grow: 1;

        &>nav {
            display: flex;
            flex-direction: column;
            padding: 2rem;
            width: 254px;
            font-size: 1rem;

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
                height: 36px;
                border: 0;
                border-radius: 0.125rem;
            }
        }
    }
</style>