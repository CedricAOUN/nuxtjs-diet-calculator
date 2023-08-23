<script setup lang="ts">

const { calories, carbs, protein } = defineProps(['calories', 'carbs', 'protein'])

function displayValue(nutrient: any) {
  return nutrient.quantity.toFixed(2) + nutrient.unit
}

</script>

<template>
  <div class="w-full h-full flex justify-center items-center ">
    <div class="flex justify-evenly w-full text-2xl shadow-md shadow-black">
      <p class="bg-red-500 w-full py-6"><b>Calories:</b> {{ calories }}</p>
      <p class="bg-orange-400 w-full py-6"><b>Carbs:</b> {{ displayValue(carbs) }}</p>
      <p class="bg-green-600 w-full py-6"><b>Protein:</b> {{ displayValue(protein) }}</p>
    </div>
  </div>
</template>

<style scoped>

</style>