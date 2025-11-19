<script lang="ts" setup>
import type { WSBTicker } from "@/App.vue";
import { computed, defineEmits } from "vue";

const { stock } = defineProps<{
  stock: WSBTicker | null;
}>();

const sentimentRotation: number = computed<number>(() => {
  const score = stock?.sentiment_score ?? 0;
  return score * 90;
});

const emit = defineEmits(["clearCurrentTicker"]);
</script>

<template>
  <div class="stock-card">
    <h2>{{ stock?.ticker }}</h2>
    <h3><a :href="'https://finance.yahoo.com/quote/' + stock?.ticker" target="_blank">View On Yahoo Finance🔗</a></h3>
  <button @click="emit('clearCurrentTicker')">Overview</button>
  <div class="gauge">
    <div
      class="needle"
      :style="{ transform: `rotate(${sentimentRotation}deg)` }"
    ></div>
  </div>
    <h2>Sentiment: {{ stock?.sentiment }}</h2>
  <h3>Sentiment Score: {{ stock?.sentiment_score }}</h3>
  </div>
</template>

<style lang="css">
.gauge {
  width: 200px;
  height: 100px;
  background: conic-gradient(var(--emerald), var(--mustard), var(--tangerine-dream));
  border-radius: 100px 100px 0 0;
  position: relative;
  overflow: hidden;
  margin-bottom: 10px;
}

.needle {
  width: 4px;
  height: 90px;
  background: black;
  position: absolute;
  left: 50%;
  bottom: 0;
  transform-origin: bottom;
}

.stock-card {
  display: flex;
  flex-direction: column;
  justify-self: center;
  align-self: center;
  align-items: center;
  padding-top: 15%;
  padding-bottom: 25%;
  gap: 1rem;
}

</style>
