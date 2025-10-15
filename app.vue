<script setup>
useHead({
  htmlAttrs: { lang: "en-US" },
  // Judul dan deskripsi diperbarui
  title: "Hypeo - Crypto Sentiment Dashboard",
  meta: [
    {
      name: "description",
      content:
        "Hypeo is your real-time dashboard for crypto market sentiment. Track market emotion and discover which hype tokens are making moves.",
    },
  ],
  bodyAttrs: {
    class: 'font-display'
  },
  // --- BAGIAN BARU YANG DITAMBAHKAN ---
  script: [
    { 
      src: 'https://accounts.google.com/gsi/client', 
      async: true, 
      defer: true 
    }
  ]
  // ------------------------------------
});
</script>

<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
