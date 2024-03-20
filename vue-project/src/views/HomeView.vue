<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" 
      v-model="searchQuery" 
      placeholder="Search for city" 
      class="py-2 px-1 w-full bg-transparent border-b border-gray-500 focus:outline-none focus:border-weather-secondary focus:outline-none focus:shadow-[0_1px_0_0_#004E71]">
    </div>

  </main>
</template>

<script setup>
const searchQuery=ref("")
</script>

