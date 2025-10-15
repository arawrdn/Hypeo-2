<script setup>
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
</script>

<template>
  <Container>
    <header class="flex flex-row flex-wrap items-center justify-between my-4 gap-x-6 gap-y-4">
      <NuxtLink to="/" class="flex items-center text-xl font-bold text-zinc-800">
        <img src="/images/hero.jpg" alt="Hypeo Logo" class="h-6 mr-2" /> 
        Hypeo
      </NuxtLink>
      
      <nav class="flex w-auto">
        <ul class="flex flex-row gap-3 text-sm">
          <li v-for="item of menuitems">
            <NuxtLink :to="item.url" class="flex px-3 py-2 text-gray-600 border rounded-sm hover:text-gray-800">
              {{ item.title }}
            </NuxtLink>
          </li>
        </ul>
      </nav>
    </header>
  </Container>
</template>
