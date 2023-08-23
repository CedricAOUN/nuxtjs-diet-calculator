<script setup lang="ts">

  interface Item {
    name: string,
    nutrition: any,
    multiplier: number,
  }

  const displayItems = ref<Item[]>(useItemList().value);

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
    <div>
      <h2>Item List</h2>
      <div class="flex flex-wrap">
        <div class="flex" v-for="i of displayItems">
          <FoodInfo :title="i.name" :quantity="i.multiplier"></FoodInfo>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>

</style>