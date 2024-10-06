<template>
    <footer>
        <h3>Ixaya Commerce</h3>
        <p>This web site it's a demo with demonstrative purposes only</p>
        <h3>Leonardo Manuel Alvarez Gonzalez</h3>
    </footer>
</template>

<script setup>

</script>

<style scoped>
    footer {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-evenly;
        padding: 1rem 2rem;
        background-color: var(--color-background-mute);
        height: 100px;
        border-width: 1px 0 0 0;
        border-style: solid;
        border-image: linear-gradient(to right, transparent, var(--color-border-1), transparent)1;
        transition: background-color 0.5s;
    }
</style>