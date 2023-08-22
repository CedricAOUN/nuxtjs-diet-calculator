<script setup lang="ts">
  const { foodItem } = defineProps(['foodItem'])
</script>

<template>
  <div class="max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 flex flex-col justify-between items-center gap-2">
    <a href="#">
      <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{{foodItem.name}}</h5>
    </a>
    <img class="object-contain object-center max-h-[150px] max-w-[150px]" :src="foodItem.image" :alt='foodItem.name + " image"'>
      <NuxtLink class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300" :to="`/foods/${foodItem.id}?type=${foodItem.foodType}`">View</NuxtLink>
  </div>
</template>

<style scoped>

</style>