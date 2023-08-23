<script setup lang="ts">

  const { foodItem } = defineProps(['foodItem'])

  let label: string = foodItem.food.label;
  let id: string = foodItem.food.foodId
  let quant: number;
  let measure: string;


  if('measures' in foodItem) {
    quant = foodItem.measures[0].weight
    measure = foodItem.measures[0].uri.split('#')[1]
  }

</script>

<template>
  <div class="max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 flex flex-col justify-between items-center gap-2">
    <a href="#">
      <h5 class="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">{{label}}</h5>
    </a>
      <NuxtLink class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300" :to="`/foods/${id}?measure=${measure}&quantity=${quant}`">View</NuxtLink>
  </div>
</template>

<style scoped>

</style>