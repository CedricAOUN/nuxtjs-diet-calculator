<script setup lang="ts">

interface Item {
  name: string,
  nutrition: Record<string, Nutrient>,
  multiplier: number,
  id: string,
}

const displayItems = ref<Item[]>(useItemList().value);

interface Nutrient {
  label: string,
  quantity: number,
  unit: string,
}


const nutrientTotals = computed(() => {
  const totals: Nutrient[] = [];

  displayItems.value.forEach((item) => {
    const nutrientRecord = item.nutrition
    const nutrientList: Nutrient[] = Object.keys(nutrientRecord).map((record) => nutrientRecord[record])
    const multiplier = item.multiplier
    console.log(nutrientList)

    nutrientList.forEach((nutrObj: any) => {
      const existingNutrient = totals.find((n) => n.label === nutrObj.label);

      if (existingNutrient) {
        existingNutrient.quantity += nutrObj.quantity * multiplier;
      } else {
        totals.push({
          label: nutrObj.label,
          quantity: nutrObj.quantity * multiplier,
          unit: nutrObj.unit,
        });
      }
    });
  });

  return totals;
});


</script>

<template>
  <section class="flex flex-col items-center py-6 gap-2">
    <h1 class="text-4xl">Results</h1>
    <div v-if="displayItems.length > 0" class="w-full" >
      <h1 class="text-center">Food item list:</h1>
      <div class="p-3 flex-col max-h-[300px] overflow-scroll w-[300px] mx-auto">
        <div v-for="i of displayItems">
          <FoodInfo class="mb-2" :title="i.name" :quantity="i.multiplier" :food-id="i.id"></FoodInfo>
        </div>
      </div>
      <h1 class="text-4xl text-center my-2">Totals</h1>
      <MainNutrition class="w-[100svw] md:w-[70svw] mx-auto text-center" :carbs="nutrientTotals.find((n)=> n.label == 'Carbohydrates (net)' )" :calories="nutrientTotals.find((n)=> n.label == 'Energy' ).quantity" :protein="nutrientTotals.find((n)=> n.label == 'Protein' )"/>
      <Totals :totals="nutrientTotals"></Totals>
    </div>
    <div v-if="displayItems.length == 0" class="flex justify-center flex-col">
      <p class="mb-5">Nothing to show! Please search for a food item and add it to the calculator</p>
      <button>
        <NuxtLink class="p-2 rounded-lg bg-green-400 text-white" to="/foods">Browse foods</NuxtLink>
      </button>
    </div>
  </section>
</template>

<style scoped>

</style>