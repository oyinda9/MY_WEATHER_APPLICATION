<script setup>
import { ref } from 'vue';
import SearchInput from './components/SearchInput.vue'
import WeatherCards from './components/WeatherCards.vue';


const places =ref([]);
const addPlace=(data)=>{
  places.value.push(data)
  console.log(data)
}
</script>


<template>
  <main>
    <!-- date -->
    <div class="text-center mb-6 text-blue-900 text-2xl">
      {{ new Date().toLocaleDateString('en-us',{
        weekday:'long',
        year:'numeric',
        month:'long',
        day:'numeric'
      }) }}

    </div>
    <!-- search -->
    <div>
      <SearchInput @place-data="addPlace"/>

    </div>
    <!-- weather card -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx">
      <WeatherCards :place="place"/>
    </div>

    </div>
    
  </main>
</template>