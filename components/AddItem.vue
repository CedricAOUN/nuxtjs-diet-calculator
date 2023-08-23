<script setup lang="ts">

const { foodData } = defineProps(['foodData'])

interface FoodObject {
  nutrition?: any,
  multiplier?: number,
  name: string,
  weight: number | undefined,
  id: string,
}

let isAdded = computed<boolean>(() => useItemList().value.some((i) => i.id === foodData.ingredients[0].parsed[0].foodId))

function handleAdd() {
  if(!isNaN(parseFloat(multiplierInput.value))) {
  let foodObject: FoodObject = {nutrition: null, multiplier: 1, name: '', weight: 0, id: ''}
  foodObject.nutrition = foodData.totalNutrients
  foodObject.multiplier = parseFloat(multiplierInput.value)
  foodObject.name = foodData.ingredients[0].parsed[0].food
  foodObject.weight = foodData.totalWeight
  foodObject.id = foodData.ingredients[0].parsed[0].foodId
  useItemList().value = [...useItemList().value, foodObject]
  }
}


function preventNonNumeric(event: any) {
  if (
      event.key === "Backspace" ||
      event.key === "ArrowLeft" ||
      event.key === "ArrowRight" ||
      event.key === "Delete" ||
      event.key === "Tab" ||
      event.key === "Enter" ||
      event.key === "."
  ) {
    return;
  }

  // Prevent input if it's not a number and not a space
  if (isNaN(Number(event.key)) && event.key !== " ") {
    event.preventDefault();
  }
}

function handleInput() {
  multiplierInput.value = multiplierInput.value.replace(' ', '')
}

const multiplierInput = ref<string>('1');
</script>

<template>
  <div v-if="!isAdded" class="flex flex-col  p-5 w-full h-full items-center justify-center gap-2">
        <h1 class="text-xl font-bold">Quantity:</h1>
        <p>1 is equal to {{foodData?.totalWeight}}g of this item.</p>
        <input @input="handleInput" v-model.number="multiplierInput" @keydown="preventNonNumeric" class="bg-gray-400 p-5 text-xl w-32 rounded-xl" placeholder="Amount of this food" type="text">
        <button @click="handleAdd" class="inline-flex items-center px-3 py-2 text-xl font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">+</button>
  </div>
  <div v-if="isAdded" class="flex-col flex gap-2">
    <p>{{(foodData?.totalWeight * parseInt(multiplierInput)).toFixed(2) }}g of this item has been added to the calculation.</p>
    <NuxtLink to="/foods" class="max-w-max mx-auto inline-flex items-center px-3 py-2 font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">Find more foods</NuxtLink>
    <NuxtLink to="/results" class="max-w-max mx-auto inline-flex items-center px-3 py-2 font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">View results</NuxtLink>
  </div>
</template>

<style scoped>

</style>