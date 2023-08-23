<script setup lang="ts">
const {nutritionList} = defineProps(['nutritionList', "calories", 'weight'])

let totalFat;
let carbs;
let protein;
let fatList = [];
let vitaminList = [];
let miscList = [];

Object.keys(nutritionList as object).map((obj) => {
  switch (nutritionList[obj].label.toLowerCase()) {
    case 'carbohydrates (net)':
      carbs = nutritionList[obj];
      break;
    case 'protein':
      protein = nutritionList[obj];
      break;
    case 'energy':
      break;
    case 'total lipid (fat)':
      totalFat = nutritionList[obj];
      break;
    case 'carbohydrate, by difference':
      break;
    default:
      if (nutritionList[obj].label.toLowerCase().startsWith('fat')) {
        fatList.push(nutritionList[obj]);
      } else if (nutritionList[obj].label.toLowerCase().startsWith('vitamin')) {
        vitaminList.push(nutritionList[obj]);
      } else {
        miscList.push(nutritionList[obj]);
      }
      break;
  }
});

function displayValue(nutrient: any) {
  return nutrient.quantity.toFixed(2) + nutrient.unit
}


</script>

<template>
  <div
       class="text-start flex flex-col items-center justify-center bg-gray-400 dark:bg-gray-800 rounded-lg p-4 max-w-max mx-auto border-2 border-gray-900 dark:border-gray-300">
    <div id="fats" class="w-full">
      <h1 class="pl-1 font-bold border-y-2 border-gray-900 dark:border-gray-300">
        Total fat: <span class="float-right pr-2">{{ displayValue(totalFat) }}</span>
      </h1>
      <p class="lastItemBorderNone ml-6 border-gray-900 dark:border-gray-300 border-b-2" v-for="n of fatList">
        <b>{{ n.label.substring(19).charAt(0).toUpperCase() + n.label.substring(20) }}:</b>
        <span class="float-right pr-2">{{ displayValue(n) }}</span>
      </p>
    </div>
    <div id="vitamins" class="w-full">
      <h1 class="pl-1 font-bold border-y-2 border-gray-900 dark:border-gray-300">Vitamins:</h1>
      <p class="lastItemBorderNone ml-6 border-gray-900 dark:border-gray-300 border-b-2" v-for="n of vitaminList">
        <b>{{ n.label.split(' ')[0] + " " + n.label.split(' ')[1].split(',')[0] }}:</b>
        <span class="float-right pr-2">{{ displayValue(n) }}</span>
      </p>
    </div>
    <div id="misc" class="w-full">
      <h1 class="pl-1 font-bold border-y-2 border-gray-900 dark:border-gray-300">Other:</h1>
      <p class="lastItemBorderNone ml-6 border-gray-900 dark:border-gray-300 border-b-2" v-for="n of miscList">
        <b>{{ n.label }}:</b>
        <span class="float-right px-2"> {{ displayValue(n) }}</span>
      </p>
    </div>
  </div>


</template>
<style scoped>
.lastItemBorderNone:last-child {
  @apply border-none
}
</style>