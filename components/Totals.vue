<script setup lang="ts">

const { totals} = defineProps(['totals'])

interface Nutrient {
  label: string,
  quantity: number,
  unit: string,
}


</script>

<template>
  <div class="bg-neutral-300 flex flex-col items-center px-20 py-4 rounded dark:bg-neutral-800">
    <h1 class="text-4xl">Totals</h1>
    <p class="text-left w-[90svw] lg:w-[30svw] border-b-[1px] border-b-black dark:border-b-white p-2" v-for="nItem of totals as Nutrient[]">{{nItem.label}} <span class="float-right"> {{nItem.quantity.toFixed(2)}}{{nItem.unit}}</span></p>
  </div>
</template>

<style scoped>

</style>