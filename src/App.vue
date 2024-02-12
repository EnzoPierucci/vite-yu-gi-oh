<script setup>


import { onMounted, ref } from 'vue';
import axios from 'axios';
import CardComponent from './components/CardComp.vue';
import LoaderComponent from './components/LoaderComp.vue';

const cards = ref([]);
const loading = ref(true);

onMounted(async () => {
  try {
    const response = await axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0');
    cards.value = response.data.data;
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }
});
</script>

<template>

  <div class="container mx-auto p-4">
    <header class="flex justify-between items-center py-2 mb-4">
      <h1 class="text-xl font-bold">Yu-Gi-Oh!</h1>
      
      <!--<select class="border p-2 rounded">
        <option value="">Select a category</option>
        
      </select>-->
    </header>

    <!-- Numero di carte trovate -->
    <div v-if="!loading && cards.length" class="mb-4">
      <span class="text-sm">Carte Trovate {{ cards.length }} </span>
    </div>

  
    <LoaderComponent v-if="loading" />

    <!-- Griglia delle carte -->
    <div v-else class="grid grid-cols-5 gap-4">
      <CardComponent 
        v-for="card in cards" 
        :key="card.id" 
        :cardData="card" />
    </div>
  </div>
  
</template>

<style scoped>

</style>
