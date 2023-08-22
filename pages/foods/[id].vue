<script setup lang="ts">

import ServingInfoModal from "~/components/ServingInfoModal.vue";

interface Item {
  image: string,
  title: string,
  name: string,
  nutrition: NutritionData,
  breadcrumbs: any[],
  servingValue: number,
  menuItem: boolean,
}


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


const {id} = useRoute().params
const {type} = useRoute().query

console.log(type);

const menuUrl = `https://api.spoonacular.com/food/menuItems/${id}?apiKey=dac835290173412eaf8ca3f8a4011a1b`
const ingrUrl = `https://api.spoonacular.com/food/ingredients/${id}/information?apiKey=dac835290173412eaf8ca3f8a4011a1b&amount=1`

const {data: item} = await useFetch<any>(type == 'menu' ? menuUrl : ingrUrl)

const i = ref<Item>(item.value)

console.log(i.value.nutrition, 'lele')

const servingValue = ref<number>(1)
let itemsList = useItemList;

const isAdded: boolean = itemsList().value.find((item: any) => {
  if (item.value.id == id) {
    return true;
  }
})


function handleAdd() {
  i.value = {...i.value, servingValue: servingValue.value}
  itemsList().value = [...itemsList().value, i]
  navigateTo('/foods')
}

const nutritionData = ref(i.value.nutrition)
// const nutritionData = computed<NutritionData>(() => i.value.nutrition);

</script>

<template>
  <section class="text-center py-16">
    <div class="flex flex-col justify-center items-center gap-3">
      <h1 class="text-4xl">{{ type == 'menu' ? i.title : i.name}}</h1>
      <img class="h-32 w-32 object-center" :src="i.image" :alt="`${i.name} - image`">
      <BreadcrumbChips :crumbs="i.breadcrumbs"/>
      <div class="flex flex-row gap-3" v-if="type == 'menu'">
        <div class="bg-green-400 dark:bg-green-600 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <p>Protein: {{ nutritionData.caloricBreakdown.percentProtein }}%</p>
          <p>Fat: {{ nutritionData.caloricBreakdown.percentFat }}%</p>
          <p>Carbs: {{ nutritionData.caloricBreakdown.percentCarbs }}%</p>
        </div>
        <div class="bg-green-400 dark:bg-green-600 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Calories</h2>
          <Icon name="fluent-mdl2:calories"></Icon>
          <p>{{ nutritionData.calories }}</p>
        </div>
        <div class="bg-green-400 dark:bg-green-600 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Carbs</h2>
          <Icon name="fa6-solid:plate-wheat"></Icon>
          <p>{{ nutritionData.carbs }}</p>
        </div>
        <div class="bg-green-400 dark:bg-green-600 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Fat</h2>
          <Icon name="mdi:trans-fat"></Icon>
          <p>{{ nutritionData.fat }}</p>
        </div>
        <div class="bg-green-400 dark:bg-green-600 flex flex-col justify-center items-center rounded flex-wrap p-5">
          <h2>Protein</h2>
          <Icon name="mdi:arm-flex"></Icon>
          <p>{{ nutritionData.protein }}</p>
        </div>


      </div>
      <div class="bg-neutral-300 px-20 py-4 rounded dark:bg-neutral-800">
        <h2 class="text-xl my-2">More Info</h2>
        <p class="text-left w-[90svw] lg:w-[30svw] border-b-[1px] border-b-black dark:border-b-white p-2"
           v-for="nItem of nutritionData.nutrients">{{ nItem.amount }}{{ nItem.unit }}<span
            class="float-right">{{ nItem.name }}</span></p>

      </div>
      <div class="max-w-[400px] flex gap-5 items-center">
        <p>Servings:</p>
        <input v-model="servingValue" class="bg-gray-400 rounded p-2" placeholder="Enter a number ex: 1.5">
        <ServingInfoModal></ServingInfoModal>
      </div>

      <button :disabled="isAdded" @click="handleAdd"
              :class="`${isAdded && 'disabled'} inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300`">
        {{ isAdded ? 'Item already added' : 'Add to calculation' }}
      </button>
    </div>
  </section>
</template>

<style scoped>
.disabled {
  @apply pointer-events-none bg-neutral-600
}


</style>