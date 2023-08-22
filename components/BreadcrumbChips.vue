<script setup lang="ts">
  const { crumbs } = defineProps(['crumbs'])
</script>

<template>
  <div class="flex flex-wrap max-w-[400px] gap-3">
    <p class="rounded-3xl bg-amber-300 dark:bg-amber-800 py-1 px-4" v-for="crumb of crumbs">{{crumb.slice(0,1).toUpperCase() + crumb.slice(1, crumb.length)}}</p>
  </div>
</template>

<style scoped>

</style>