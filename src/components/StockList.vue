<script lang="ts" setup>
    import type { WSBTicker } from '@/App.vue';
    import { defineEmits } from 'vue';

    const props = defineProps<{
        stocks: WSBTicker[],
        title: String,

    }>();

    const emit = defineEmits(['chooseTicker']);

</script>

<template>
    <div class="stock-list">
        <h2>{{ title }}</h2>
    <ul>
        <li v-for="stock in stocks" :key="stock.ticker">
            <h2>{{ stock.ticker }}</h2>
            <button @click="emit('chooseTicker', stock)">View Info</button>
        </li>
    </ul>
    </div>
</template>

<style>
    ul {
        margin-left: 25%;
        margin-right: 25%;
    }

    li {
        border: 1px solid var(--dark-slate-grey);
    }

    li > h2 {
        border-bottom: 1px solid var(--dark-slate-grey);
    }
</style>