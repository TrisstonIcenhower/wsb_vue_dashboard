<script setup lang="ts">
import axios from "axios";
import { computed, reactive, ref, type Ref } from "vue";
import WSBOverview from "./components/WSBOverview.vue";
import StockView from "./components/StockView.vue";
import WSBSummary from "./components/WSBSummary.vue";

export interface WSBTicker {
  ticker: string;
  sentiment: string;
  sentiment_score: number;
  no_of_comments: number;
}

export interface OverviewStats {
  numBullishTickers: number;
  numBearishTickers: number;
  numTotalTickers: number;
  totalComments: number;
}

async function getInfo() {
  await axios
    .get<WSBTicker[]>("https://api.tradestie.com/v1/apps/reddit")
    .then((response) => {
      bullishTickers.value = response.data.filter(
        (ticker) => ticker.sentiment === "Bullish"
      );
      bearishTickers.value = response.data.filter(
        (ticker) => ticker.sentiment === "Bearish"
      );
    });
}

function setTicker(currentTicker: WSBTicker, stock: WSBTicker){
  currentTicker.ticker = stock.ticker;
  currentTicker.sentiment = stock.sentiment;
  currentTicker.sentiment_score = stock.sentiment_score;
  currentTicker.no_of_comments = stock.no_of_comments;
}

function clearTicker(currentTicker: WSBTicker){
  currentTicker.ticker = '';
  currentTicker.sentiment = '';
  currentTicker.sentiment_score = 0;
  currentTicker.no_of_comments = 0;
}

let bullishTickers: Ref<WSBTicker[]> = ref([]);
let bearishTickers: Ref<WSBTicker[]> = ref([]);
let currentTicker: Ref<WSBTicker> = ref<WSBTicker>({
  ticker: '', sentiment: '', sentiment_score: 0, no_of_comments: 0
});

let overview: OverviewStats = reactive<OverviewStats>({
  numBearishTickers: 0,
  numTotalTickers: 0,
  numBullishTickers: 0,
  totalComments: 0,
});

getInfo().then(() => {
  (overview.numBullishTickers = bullishTickers.value?.length ?? 0),
    (overview.numBearishTickers = bearishTickers.value?.length ?? 0),
    (overview.numTotalTickers =
      (bullishTickers.value?.length ?? 0) +
      (bearishTickers.value?.length ?? 0)),
    (overview.totalComments =
      bullishTickers.value.reduce((a, s) => a + s.no_of_comments, 0) +
      bearishTickers.value.reduce((a, s) => a + s.no_of_comments, 0));
});

const displayType = computed(() => {
  if (currentTicker.value.ticker === '') {
    return "main-overview";
  } else {
    return "main-stock-view";
  }
});
</script>

<template>
  <main :class="displayType">
    <header class="header"><h1>Wall Street Bets Dashboard</h1></header>
    <div v-if="currentTicker.ticker === ''" class="breakdown">
      <WSBSummary :overview="overview"></WSBSummary>
    </div>
    <WSBOverview
      v-if="currentTicker.ticker === ''"
      :bear-stocks="bearishTickers"
      ,
      :bull-stocks="bullishTickers"
      @choose-ticker="(stock) => (setTicker(currentTicker, stock))"
    ></WSBOverview>
    <StockView
      v-else
      :stock="currentTicker"
      @clear-current-ticker="clearTicker(currentTicker)"
    ></StockView>
  </main>
</template>

<style scoped>
header {
  font-size: large;
}
</style>
