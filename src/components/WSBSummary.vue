<script setup lang="ts">
import type { OverviewStats } from "@/App.vue";

const props = defineProps<{
  overview: OverviewStats;
}>();

</script>

<template>
  <div class="wsb-summary">
    <h2>Top 50 Stocks Mentioned on WSB</h2>
    <div
      class="pie"
      :style="{
        backgroundImage: `conic-gradient(var(--emerald) ${
          (overview.numBullishTickers / overview.numTotalTickers) * 100
        }%, var(--tangerine-dream) ${
          (overview.numBearishTickers / overview.numTotalTickers) * 100
        }% )`,
      }"
    ></div>
    <div class="pie-info"><div style="width: 10px; height: 10px; background-color: var(--emerald); border-radius: 50%;"></div><h2>Bullish Stocks {{ (overview.numBullishTickers / overview.numTotalTickers) * 100 }}%</h2></div>
    <div class="pie-info"><div style="width: 10px; height: 10px; background-color: var(--tangerine-dream); border-radius: 50%;"></div><h2>Bearish Stocks {{ (overview.numBearishTickers / overview.numTotalTickers) * 100 }}%</h2></div>
    <div class="pie-info">Total Comments Reported: {{ overview.totalComments }}</div>
    <div class="pie-info">Info gathered from <a href="https://tradestie.com/apps/reddit/api/" target="_blank">tradestie.com</a></div>
  </div>
</template>

<style lang="css">
.wsb-summary {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.pie {
  width: clamp(50px, 200px, 400px);
  height: clamp(50px, 200px, 400px) ;
  border-radius: 50%;
  border: 5px solid var(--pacific-blue);
}

.pie-info {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
</style>
