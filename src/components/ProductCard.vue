<template>
    <div>
        <h2>{{ product?.title }}</h2>
        <textarea>{{ product?.description }}</textarea>
    </div>
</template>

<script setup>
    defineProps({
        product: Object
    })
</script>

<style scoped>
    
</style>