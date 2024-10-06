<template>
    <header>
        <nav>
            <RouterLink to="/"><h1>Ixaya Commerce</h1></RouterLink>
            <RouterLink to="/orders"><ShoppingCart /><span>Carrito</span></RouterLink>
        </nav>
    </header>

    <RouterView />
</template>

<script setup>
    import { RouterLink, RouterView } from 'vue-router'
    import ShoppingCart from '../components/icons/ShoppingCart.vue'
</script>

<style scoped>
    header {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        position: sticky;
        top: 0;
        height: 60px;
        border-width: 0 0 1px 0;
        border-style: solid;
        border-image: linear-gradient(to right, transparent, var(--color-background-mute), transparent)1;
        background-color: var(--accent-color);
        transition: background-color 0.5s;
        padding: 0 2rem;
        z-index: 99;

        &>nav {
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: space-between;
            width: 100%;
            height: 100%;

            &>a {
                display: flex;
                flex-direction: row;
                align-items: center;
                justify-content: center;
                height: 100%;
                border-radius: 0.25rem;

                &>h1 {
                    font-weight: bold;
                    color: var(--color-heading);
                    transition: color 0.5s;
                }

                &>span {
                    display: flex;
                    justify-content: center;
                    align-items: end;
                    height: 40px;
                    font-weight: bold;
                    color: var(--color-heading);
                    transition: color 0.5s;
                }
            }
        }
    }
</style>