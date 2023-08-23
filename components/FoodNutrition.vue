<script setup lang="ts">
const { nutritionList } = defineProps(['nutritionList'])

</script>

<template>
<div>
  <div v-for="i of Object.keys(nutritionList as object)">{{nutritionList[i].label}}: {{nutritionList[i].quantity.toFixed(2)}} {{nutritionList[i].unit}}</div>
</div>
</template>

<style scoped>

</style>