<script setup>
import { ref, onMounted } from 'vue';
import pageData from '@/data/index.json'

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
    // ---- PERUBAHAN UTAMA ADA DI SINI ----
    // Kita memindahkan API Key ke dalam 'headers'
    const response = await fetch(URL, {
      headers: {
        'x-cg-demo-api-key': API_KEY
      }
    });
    // ------------------------------------

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
