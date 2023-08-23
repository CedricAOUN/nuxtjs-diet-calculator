<script setup lang="ts">

import AddItem from "~/components/AddItem.vue";

const { id } = useRoute().params;
const { measure, quantity } = useRoute().query;


const reqBody = {
  "ingredients": [
    {
      "foodId": `${id}`,
      "weight": parseInt(quantity as string),
      "measureURI": `http://www.edamam.com/ontologies/edamam.owl#${measure}`
    }
  ]
};

const apiUrl = `https://api.edamam.com/api/food-database/v2/nutrients?app_id=c7d1b491&app_key=f60db3c5476f4f0c68beb07c74ab65d1`;

interface FoodData {
  calories: number;
  cautions: string[];
  dietLabels: string[];
  healthLabels: string[];
  totalWeight: number;
  totalNutrients: any;
  totalDaily: any;
  ingredients: any[];
}

let foodData = ref<FoodData | null>(null);

let foodName = foodData.value?.ingredients[0]?.parsed[0]?.food

console.log(foodName)

const fetchFoodData = async () => {
  try {
    const response = await useFetch(apiUrl, {
      method: 'post',
      body: reqBody
    });

    foodData.value = response.data.value as FoodData;
    console.log(foodData.value)
  } catch (error) {
    console.log('Error getting food data', error);
  }
};


//TODO fish issue with regular fish item not working


onMounted(fetchFoodData);
watch(() => id, fetchFoodData);

</script>

<template>
  <section class="text-center py-16">
    <div v-if="foodData != null" class="grid grid-cols-3 grid-rows-5 gap-4 px-16">
      <div class="col-span-2 row-span-2 flex justify-center items-center text-6xl font-extrabold"><h1>{{foodData?.ingredients[0]?.parsed[0]?.food.charAt(0).toUpperCase() + foodData?.ingredients[0]?.parsed[0]?.food.slice(1) }}</h1></div>
      <div class="row-span-5 col-start-3 flex justify-center items-center "><FoodNutrition :nutrition-list="foodData.totalNutrients" :calories="foodData.calories" :weight="foodData.totalWeight"/></div>
      <div class="col-span-2 row-start-3 flex justify-center items-center "><MainNutrition :calories="foodData.calories" :carbs="foodData.totalNutrients['CHOCDF.net']" :protein="foodData.totalNutrients['PROCNT']"/></div>
      <div class="col-span-2 row-span-2 row-start-4 flex justify-center items-center "><AddItem :food-data="foodData"/></div>
    </div>

  </section>
</template>

<style scoped>
.disabled {
  @apply pointer-events-none bg-neutral-600
}


</style>