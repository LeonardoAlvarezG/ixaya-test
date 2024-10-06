<script setup>
    import NewestProductList from '../components/NewestProductList.vue'
</script>

<template>
    <section class="section">
        <article>
            <section>
                <h1>Compra Los Mejores Productos De Todo El Mundo</h1>
                <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Odit dolor odio consectetur illo omnis vero autem dolores inventore voluptatem, porro, quam nemo explicabo voluptas possimus incidunt distinctio ab veritatis veniam!</p>
                <router-link to="/products">Productos</router-link>
            </section>
            <figure>
                <!-- <img loading="lazy" src="../assets/shoppingboxes.png" alt="Shopping Boxes" decoding="async" /> -->
                <img loading="lazy" src="../assets/group-shopping-bags.webp" alt="Shopping Boxes" decoding="async" width="932" height="448" />
            </figure>
        </article>
        <NewestProductList />
    </section>
</template>

<style scoped>
section.section {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    
    &>article {
        position: relative;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        min-height: 50vh;
        height: 100%;
        padding: 1rem 2rem;
        background-color: var(--accent-color);

        &>section {
            display: flex;
            flex-direction: column;
            align-items: start;
            justify-content: center;
            min-width: 20vw;
            width: 35vw;

            &>h1 {
                font-size: 2.5rem;
                color: var(--color-heading);
                font-weight: bold;
                margin: 1rem 0;
                transition: color 0.5s;
            }

            &>p {
                margin: 1rem 0;
            }

            &>a {
                display: flex;
                align-items: center;
                justify-content: center;
                height: 60px;
                padding: 0 2rem;
                margin: 1rem 0;
                border-radius: 30px;
                font-size: 1.125rem;
                font-weight: bold;
                line-height: unset;
                background-color: var(--color-background);
            }
        }

        &>figure {
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: end;
            flex-grow: 1;
            padding: 2rem 6.25rem;
            max-height: 40vh;

            &>img {
                object-fit: contain;
                width: 100%;
                filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.7));
                aspect-ratio: 1;
            }
        }

        &::before {
            content: '';
            position: absolute;
            bottom: 0;
            width: 0;
            height: 0;
            border-style: solid;
            border-width: 0 44px 44px 44px;
            border-color: transparent transparent var(--color-background) transparent;
            left: 50%;
            transform: translateX(-50%);
            transition: border-color 0.5s;
        }
    }
}
</style>
