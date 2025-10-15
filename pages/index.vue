<script setup>
import { ref, onMounted } from 'vue';
import pageData from '../data/index.json' // Path diperbaiki

const page = ref(pageData);
const trendingCoins = ref([]);
const isLoading = ref(true);
const error = ref(null);

definePageMeta({
  layout: "default",
});

const fetchTrendingCoins = async () => {
  const API_KEY = page.value.config.coingeckoApiKey;
  const URL = 'https://api.coingecko.com/api/v3/search/trending';
  
  try {
    // Memanggil API dengan key di header untuk stabilitas
    const response = await fetch(URL, {
      headers: {
        'x-cg-demo-api-key': API_KEY
      }
    });

    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    trendingCoins.value = data.coins.slice(0, 10); 
  } catch (err) {
    error.value = "Failed to load trending data. Check API key or network connection.";
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  fetchTrendingCoins();
});
</script>

<template>
  <Container>
    <Hero
      :title="page.hero.title"
      :description="page.hero.description"
      :image="page.hero.image"
      :imageAlt="page.hero.imageAlt"
      :buttons="page.hero.buttons"
    ></Hero>

    <section id="trending-data" class="py-12">
      <h2 class="text-3xl font-bold mb-6 text-center">New Hot Entries (Top 10)</h2>
      
      <div v-if="isLoading" class="text-center text-gray-500">Loading live market data...</div>
      <div v-else-if="error" class="text-center text-red-500">{{ error }}</div>
      
      <div v-else class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
        <div v-for="coin in trendingCoins" :key="coin.item.id" class="p-4 border rounded-lg shadow-sm">
          <div class="flex items-center mb-2">
            <img :src="coin.item.small" :alt="coin.item.name" class="w-6 h-6 mr-3 rounded-full" />
            <h3 class="font-semibold">{{ coin.item.name }} ({{ coin.item.symbol }})</h3>
          </div>
          <p class="text-sm text-gray-600">Market Cap Rank: #{{ coin.item.market_cap_rank }}</p>
          <p class="text-sm text-gray-600">Price (BTC): {{ coin.item.price_btc.toFixed(8) }}</p>
          <a :href="`https://www.coingecko.com/en/coins/${coin.item.id}`" target="_blank" class="text-blue-500 text-xs mt-2 inline-block">View on CoinGecko &rarr;</a>
        </div>
      </div>
      <div v-if="trendingCoins.length === 0 && !isLoading && !error" class="text-center text-gray-500">No coins are currently trending.</div>
    </section>
    
    <Logos :title="page.logos.title" :icons="page.logos.icons"></Logos>
    <Features
      :title="page.features.title"
      :description="page.features.description"
      :items="page.features.items"
    ></Features>
    <Testimonials
      :title="page.testimonials.title"
      :description="page.testimonials.description"
      :items="page.testimonials.items"
    ></Testimonials>
    <Cta
      :title="page.cta.title"
      :description="page.cta.description"
      :buttons="page.cta.buttons"
    ></Cta>
  </Container>
</template>

<style scoped>
.items-center { display: flex; align-items: center; }
.w-6 { width: 1.5rem; }
.h-6 { height: 1.5rem; }
.mr-3 { margin-right: 0.75rem; }
.rounded-full { border-radius: 9999px; }
.grid { display: grid; }
.md\:grid-cols-2 { grid-template-columns: repeat(2, minmax(0, 1fr)); }
.lg\:grid-cols-4 { grid-template-columns: repeat(4, minmax(0, 1fr)); }
.gap-6 { gap: 1.5rem; }
.py-12 { padding-top: 3rem; padding-bottom: 3rem; }
.text-3xl { font-size: 1.875rem; line-height: 2.25rem; }
.font-bold { font-weight: 700; }
.mb-6 { margin-bottom: 1.5rem; }
.text-center { text-align: center; }
.p-4 { padding: 1rem; }
.border { border-width: 1px; }
.rounded-lg { border-radius: 0.5rem; }
.shadow-sm { box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); }
.mb-2 { margin-bottom: 0.5rem; }
.font-semibold { font-weight: 600; }
.text-sm { font-size: 0.875rem; line-height: 1.25rem; }
.text-gray-600 { color: #4b5563; }
.text-blue-500 { color: #3b82f6; }
.text-xs { font-size: 0.75rem; line-height: 1rem; }
.mt-2 { margin-top: 0.5rem; }
.inline-block { display: inline-block; }
</style>
