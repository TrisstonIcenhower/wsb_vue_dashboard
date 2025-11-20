<script lang="ts" setup>
    import type { WSBTicker } from '@/App.vue';
    import StockList from './StockList.vue';
    
    const props = defineProps<{
        bullStocks: WSBTicker[],
        bearStocks: WSBTicker[],
    }>();

    const emit = defineEmits(['chooseTicker']);
</script>

<template>
    <StockList :stocks="bullStocks" :title="'🐂Bullish'" @choose-ticker="(stock) => emit('chooseTicker', stock)"></StockList>
    <StockList :stocks="bearStocks" :title="'🐻Bearish'" @choose-ticker="(stock) => emit('chooseTicker', stock)"></StockList>
</template>

<style>

</style>