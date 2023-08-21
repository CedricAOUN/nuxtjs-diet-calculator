<script setup lang="ts">

interface CaloricBreakdown {
  percentProtein: number,
  percentFat: number,
  percentCarbs: number
}
interface NutritionData {
  caloricBreakdown: CaloricBreakdown,
  calories: number,
  carbs: string,
  fat: string,
  nutrients: any[],
  protein: string,
}


 const { id } = useRoute().params

 const url = `https://api.spoonacular.com/food/menuItems/${id}?apiKey=dac835290173412eaf8ca3f8a4011a1b`;
 const { data: item } = await useFetch<any>(url)

 const i = ref<any>(item.value)

console.log(i);

const nutritionData = computed<NutritionData>(() => i.value.nutrition);

</script>

<template>
  <section class="text-center py-16">
    <div class="flex flex-col justify-center items-center gap-3">
      <h1 class="text-4xl">{{ i.title }}</h1>
      <img class="h-32 w-32 object-center" :src="i.image" :alt="`${i.title} - image`">
      <!-- Generate chips for breadcrumbs TODO-->
      <div class="flex flex-row gap-3">
        <div class="bg-green-400 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <p>Protein: {{nutritionData.caloricBreakdown.percentProtein}}%</p>
          <p>Fat: {{nutritionData.caloricBreakdown.percentFat}}%</p>
          <p>Carbs: {{nutritionData.caloricBreakdown.percentCarbs}}%</p>
        </div>
        <div class="bg-green-400 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Calories</h2>
          <Icon name="fluent-mdl2:calories"></Icon>
          <p>{{nutritionData.calories}}</p>
        </div>
        <div class="bg-green-400 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Carbs</h2>
          <Icon name="fa6-solid:plate-wheat"></Icon>
          <p>{{nutritionData.carbs}}</p>
        </div>
        <div class="bg-green-400 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Fat</h2>
          <Icon name="mdi:trans-fat"></Icon>
          <p>{{nutritionData.fat}}</p>
        </div>
        <div class="bg-green-400 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Protein</h2>
          <Icon name="mdi:arm-flex"></Icon>
          <p>{{nutritionData.protein}}</p>
        </div>


      </div>
      <div class="bg-neutral-300 px-20 py-4 rounded dark:bg-neutral-800">
        <h2 class="text-xl my-2">More Info</h2>
        <p class="text-left w-[90svw] lg:w-[30svw] border-b-[1px] border-b-black dark:border-b-white p-2" v-for="nItem of nutritionData.nutrients">{{nItem.amount}}{{nItem.unit}}<span class="float-right">{{nItem.name}}</span></p>

      </div>

      <!-- TODO: add serving input and make the info notice a modal-->
      <div class="max-w-[400px]"><input placeholder="Enter a number ex: 1.5">
        <p>{{
            `Enter a serving size. In other words, the quantity of this food that you would like the calculation to consider. A single serving is equal to 1.
      For whole foods, you will need to follow the guidelines to determine what is "1" serving, as shown below:`
          }}</p>
        <img class="h-[700px]"
             src="https://reallifegoodfood.umn.edu/sites/reallifegoodfood.umn.edu/files/2022-03/portion-size-gu_37746735.png"
             alt="portion size guidelines">
        <p>{{ `For menu items, the item itself is considered a single serving.` }}</p></div>

      <button class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">
      Add to calculator
      </button>
    </div>
  </section>
</template>

<style scoped>

</style>