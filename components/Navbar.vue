<script setup>
import { ref } from 'vue';

const isMenuOpen = ref(false);

const menuitems = [
  {
    title: "Sign up",
    url: "/signup",
  },
  {
    title: "Log in",
    url: "/login",
  },
];

function toggleMenu() {
  isMenuOpen.value = !isMenuOpen.value;
}
</script>

<template>
  <Container>
    <header class="relative flex flex-wrap items-center justify-between my-4">
      <NuxtLink to="/" class="flex items-center text-xl font-bold text-zinc-800">
        <img src="images/hypeo.jpg" alt="Hypeo Logo" class="h-6 mr-2" />
        Hypeo
      </NuxtLink>

      <button @click="toggleMenu" class="md:hidden p-2 rounded-md text-zinc-600 hover:bg-zinc-100 focus:outline-none">
        <svg v-if="!isMenuOpen" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>

      <nav :class="['w-full md:flex md:items-center md:w-auto', { 'hidden': !isMenuOpen }]" class="mt-4 md:mt-0">
        <ul class="flex flex-col md:flex-row gap-3 text-sm">
          <li v-for="item of menuitems" class="w-full md:w-auto">
            <NuxtLink @click="isMenuOpen = false" :to="item.url" class="flex justify-center px-3 py-2 text-gray-600 border rounded-sm hover:text-gray-800 w-full">
              {{ item.title }}
            </NuxtLink>
          </li>
        </ul>
      </nav>
    </header>
  </Container>
</template>
