<template>
    <div v-if="bought">
        <span>{{ order?.street_name }}</span>
        <span>{{ order?.zip_code }}</span>
        <span>{{ order?.address }}</span>
        <span>{{ order?.phone }}</span>
        <span>{{ order?.state }}</span>
        <span>{{ order?.city }}</span>
    </div>
    <div v-else>
        No Comprado
    </div>
</template>

<script setup>
    defineProps({
        order: Object,
        bought: Boolean
    })
</script>

<style scoped>
    
</style>