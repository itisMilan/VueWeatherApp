<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for city"
        class="py-2 px-1 w-full bg-transparent border-b  focus:outline-none focus:border-weather-secondary focus:outline-none focus:shadow-[0_1px_0_0_#004E71]">
    </div>
    <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[146px] "
      v-if="mapboxSearchResults">
      <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id" class="py-2 cursor-pointer">
        {{ searchResult.place_name }}
      </li>
      <p v-if="searchError">Sorry, something went wrong, please try again.</p>
      <p v-if="!searchError && !mapboxSearchResults">No results match your query, try a different term.</p>
      <template v-else>
        <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id" class="py-2 cursor-pointer">
          {{ searchResult.place_name }}</li>
      </template>
    </ul>

  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
const mapboxApiKey = import.meta.env.VITE_APP_PUBLIC_KEY_WEATHER;
console.log(mapboxApiKey)
const searchQuery = ref("")
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);

const getSearchResults = () => {
  console.log(mapboxApiKey)
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxApiKey}&types=place`)
        mapboxSearchResults.value = result.data.features;

      }
      catch {
        searchError.value = true;
      }
      return;
    }
    mapboxSearchResults.value = null
    console.log(mapboxSearchResults.value)
  }, 300)
}
</script>
