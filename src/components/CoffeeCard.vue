<script setup>
    import { computed } from 'vue';

    const { coffee } = defineProps({
        coffee: {
            type: Object,
            required: true,
        }
    });

    const ratingImage = computed(() => {
        return coffee.rating && coffee.votes
            ? '/src/assets/Star_fill.svg'
            : '/src/assets/Star.svg';
    });

</script>

<template>
    <article>
        <img :src="coffee.image" alt="" width="100%" class="coffee-image">
        <span v-if="coffee.popular" class="popular-badge">Popular</span>

        <div class="flex-row">
            <span>{{ coffee.name }}</span>
            <span class="price-tag">{{ coffee.price }}</span>
        </div>

        <div class="flex-row">
            <p class="rating">
                <img :src="ratingImage" alt="" width="24px" height="24px">
                <span v-if="coffee.rating">{{ coffee.rating }}</span>
                
                <p class="votes">
                    <span v-if="coffee.votes">({{ coffee.votes }} votes)</span>
                    <span v-else>No ratings</span>
                </p>
            </p>

            <span v-if="!coffee.available" class="availability">Sold out</span>
        </div>
    </article>
</template>

<style scoped>

article{
    position: relative;

    display: flex;
    flex-direction: column;
    row-gap: 15px;

    img.coffee-image{
        width: 100%;
        border-radius: 13px;
    }

    .popular-badge{
        position: absolute;
        top: 10px;
        left: 10px;

        background-color: var(--popularity-color);
        border-radius: 13px;

        width: fit-content;
        padding: 2px 10px;

        color: var(--background);
        text-align: center;
        font-weight: 600;
        font-size: 12px;
    }

    p{
        margin: 0;
    }
    
    .flex-row{
        display: flex;
        align-items: center;
        justify-content: space-between;

        font-size: 16px;
        font-weight: 500;
    }

    span.price-tag{
        background-color: var(--price-color);
        color: var(--background);
        padding: 3px 8px;
        border-radius: 5px;

        font-size: 14px;
    }

    .rating{
        display: flex;
        align-items: center;
        column-gap: 5px;

        font-size: 14px;
    }

    .votes{
        color: var(--gray)
    }

    .availability{
        color: var(--sold-out-color);
        font-weight: initial;
    }

}

</style>