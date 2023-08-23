<script setup lang="ts">

const { totals} = defineProps(['totals'])

interface Nutrient {
  label: string,
  quantity: number,
  unit: string,
}


</script>

<template>
  <div class="flex flex-col items-center md:px-20 py-4 rounded">
    <FoodNutrition :nutrition-list="totals" />
  </div>
</template>

<style scoped>

</style>