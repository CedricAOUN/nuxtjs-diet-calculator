<script setup lang="ts">

const { foodData } = defineProps(['foodData'])

interface FoodObject {
  nutrition?: any,
  multiplier?: number,
  name: string,
  weight: number | undefined,
}
function handleAdd() {
  let foodObject: FoodObject = {nutrition: null, multiplier: 1, name: '', weight: 0}
  foodObject.nutrition = foodData.totalNutrients
  foodObject.multiplier = multiplierInput.value
  foodObject.name = foodData.ingredients[0].parsed[0].food
  foodObject.weight = foodData.totalWeight
  useItemList().value = [...useItemList().value, foodObject]
  console.log(useItemList().value)
}

const multiplierInput = ref<number>(1);
</script>

<template>
  <div class="flex flex-col  p-5 w-full h-full items-center justify-center gap-2">
        <h1 class="text-xl font-bold">Quantity:</h1>
        <p>1 is equal to {{foodData?.totalWeight}}g of this item.</p>
        <input class="bg-gray-400 p-5 text-xl w-16 rounded-xl" placeholder="Amount of this food" v-model="multiplierInput">
        <button @click="handleAdd" class="inline-flex items-center px-3 py-2 text-xl font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">+</button>
  </div>
</template>

<style scoped>

</style>