<script setup>

import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import CardComponent from './components/CardComp.vue';
import LoaderComponent from './components/LoaderComp.vue';


const selectedArchetype = ref('');
const archetypes = ref([]);
const cards = ref([]);
const loading = ref(false);

const fetchArchetypes = async () => {
  try {
    const response = await axios.get('https://db.ygoprodeck.com/api/v7/archetypes.php');
    archetypes.value = response.data;
  } catch (error) {
    console.error(error);
  }
};

const fetchCards = async () => {
  loading.value = true;
  try {
    const response = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php`, {
      params: {
        archetype: selectedArchetype.value,
        num: 20,
        offset: 0,
      }
    });
    cards.value = response.data.data;
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }
};

onMounted(fetchArchetypes);

watch(selectedArchetype, (newValue, oldValue) => {
  if (newValue !== oldValue) {
    fetchCards();
  }
});

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
      
      <select v-model="selectedArchetype" @change="fetchCards" class="border p-2 rounded">
        <option value="">Archetipi</option>
        <option v-for="archetype in archetypes" :key="archetype.id" :value="archetype.name">
          {{ archetype.name }}
        </option>
      </select>
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
