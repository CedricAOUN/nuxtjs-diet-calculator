<script setup lang="ts">

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

interface FoodObject {
  nutrition?: any,
  multiplier?: number,
  name: string,
  weight: number | undefined,
}

function handleAdd() {
  let foodObject: FoodObject = {nutrition: null, multiplier: 1, name: '', weight: 0}
  foodObject.nutrition = foodData.value?.totalNutrients
  foodObject.multiplier = multiplierInput.value
  foodObject.name = foodData.value?.ingredients[0].parsed[0].food
  foodObject.weight = foodData.value?.totalWeight
  useItemList().value = [...useItemList().value, foodObject]
  console.log(useItemList().value)
}

const multiplierInput = ref<number>(1);

onMounted(fetchFoodData);
watch(() => id, fetchFoodData);

</script>

<template>
  <section class="text-center py-16">
    <div v-if="foodData != null">
      <h1>Hello {{ id }}</h1>
        <!-- Placeholder calorie component-->
      <p>Calories per serving: {{ foodData.calories }}</p>
      <p>Weight per serving: {{foodData.totalWeight}}g</p>
      <FoodNutrition :nutrition-list="foodData.totalNutrients"></FoodNutrition>
      <p>How much of this food?</p>
      <input placeholder="Amount of this food" v-model="multiplierInput">
      <button @click="handleAdd">Add to calculator</button>
    </div>
  </section>
</template>

<style scoped>
.disabled {
  @apply pointer-events-none bg-neutral-600
}


</style>