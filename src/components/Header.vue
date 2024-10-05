<template>
    <header>
        <nav>
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/orders">Shopping Cart</RouterLink>
        </nav>
    </header>

    <RouterView />
</template>

<script setup>
    import { RouterLink, RouterView } from 'vue-router'
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
            }
        }
    }
</style>