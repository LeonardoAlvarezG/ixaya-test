<template>
    <article>
        <section>
            <h1>Buy The Best Products From All The World</h1>
            <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Odit dolor odio consectetur illo omnis vero autem dolores inventore voluptatem, porro, quam nemo explicabo voluptas possimus incidunt distinctio ab veritatis veniam!</p>
            <router-link to="/products">Products</router-link>
        </section>
        <figure>
            <img src="../assets/shoppingboxes.png" alt="Shopping Boxes" />
        </figure>
    </article>
    <section>
        <h2>Newest Products</h2>
        <ul>
            <li>Product 1</li>
            <li>Product 2</li>
            <li>Product 3</li>
            <li>Product 4</li>
            <li>Product 5</li>
        </ul>
    </section>
</template>

<script>

</script>

<style scoped>
    article {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        background-color: darkblue;
        height: 1080px;

        & h1 {
            font-size: 2em;
        }
    }
</style>