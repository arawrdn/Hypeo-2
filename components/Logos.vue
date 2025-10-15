<script setup>
const props = defineProps({
  title: String,
  icons: Array
});
</script>

<template>
  <div class="mb-20">
    <h2 class="text-center text-zinc-600" v-if="title">{{ title }}</h2>
    <div class="flex flex-wrap items-center justify-center mt-10 gap-x-12 gap-y-8 sm:gap-x-24 sm:gap-y-12">
      <div v-for="icon of icons" :key="icon.url">
        <img 
          class="h-8 md:h-12" 
          v-if="icon.url" 
          :src="icon.url" 
          :alt="icon.url" 
        />
        </div>
    </div>
  </div>
</template>
