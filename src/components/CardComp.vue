<script setup>
defineProps(['cardData']);
</script>

<template>
  <div class="border rounded overflow-hidden shadow-lg">
    <img :src="cardData.card_images[0].image_url" class="w-full" alt="Card image" />
    <div class="p-4">
      <h3 class="font-bold">{{ cardData.name }}</h3>
      <p class="text-xs">{{ cardData.type }}</p>
    </div>
  </div>
</template>

<style scoped>

</style>
