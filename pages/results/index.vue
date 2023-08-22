<script setup lang="ts">
  const itemList = useItemList();
  const displayItems = ref(itemList.value);

  interface Nutrient {
    name: string,
    amount: number,
    unit: string,
  }



  const nutrientTotals = computed(() => {
    const totals: Nutrient[] = [];

    displayItems.value.forEach((item) => {
      const nutrientList = item.value.nutrition.nutrients;
      const multiplier = item.value.servingValue;

      nutrientList.forEach((nutrObj: any) => {
        const existingNutrient = totals.find((n) => n.name === nutrObj.name);

        if (existingNutrient) {
          existingNutrient.amount += nutrObj.amount * multiplier;
        } else {
          totals.push({
            name: nutrObj.name,
            amount: nutrObj.amount * multiplier,
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
    <div class="flex flex-wrap gap-4">
      <FoodInfo v-for="i of displayItems" :title="i.value.title" :id="i.value.id" :quantity="i.value.servingValue"></FoodInfo>
    </div>
    <div>
      <Totals v-if="nutrientTotals.length > 0" :totals="nutrientTotals"></Totals>
      <p v-if="nutrientTotals.length == 0">Nothing to calculate! Browse the food section, and add something to be calculated.</p>
    </div>
  </section>
</template>

<style scoped>

</style>