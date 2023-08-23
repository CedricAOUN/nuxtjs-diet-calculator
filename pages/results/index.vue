<script setup lang="ts">

  interface Item {
    name: string,
    nutrition: Record<string, Nutrient>,
    multiplier: number,
  }

  const displayItems = ref<Item[]>(useItemList().value);

  interface Nutrient {
    label: string,
    quantity: number,
    unit: string,
  }

  console.log(displayItems.value[0].nutrition)


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
    <div>
      <h2>Item List</h2>
      <div class="flex flex-wrap">
        <div class="flex" v-for="i of displayItems">
          <FoodInfo :title="i.name" :quantity="i.multiplier"></FoodInfo>
        </div>
      </div>
      <div>
        <h2>Info</h2>
        <Totals :totals="nutrientTotals"></Totals>
      </div>
    </div>
  </section>
</template>

<style scoped>

</style>