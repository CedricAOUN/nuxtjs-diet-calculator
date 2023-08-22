<script setup lang="ts">
  const open = ref(false);

  function handleOpen() {
    open.value = !open.value
    if(open.value) {
      document.documentElement.style.overflow = 'hidden';
    } else {
      document.documentElement.style.overflow = 'auto';
    }
  }

</script>

<template>

  <!-- Modal toggle -->
  <button @click="handleOpen" class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300">
    What is a serving?
  </button>

  <!-- Main modal -->
  <div :class="`fixed top-0 left-0 right-0 z-50 ${open ? '' : 'hidden'} w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full px-1 py-1`">
    <div class="flex flex-col justify-center items-center w-full bg-gray-200 dark:bg-gray-900 p-4 gap-1 rounded-lg">
      <p class="w-1/2">{{
        `A serving is a standardized portion of a food item. To determine your serving intake, refer to the portion size chart. This chart offers visual cues to help you estimate portion sizes for different whole food items. Use it to determine how much of a certain food should the calculation
        take into account.`
      }}</p>
      <p> {{`Note: For items with the tag "Menu Item type", the item itself is 1 serving`}}</p>
      <img class="h-[700px]"
           src="https://reallifegoodfood.umn.edu/sites/reallifegoodfood.umn.edu/files/2022-03/portion-size-gu_37746735.png"
           alt="portion size guidelines">
      <button class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-green-400 rounded-lg hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 dark:bg-green-600 dark:hover:bg-green-400 dark:focus:ring-green-300" @click="handleOpen"> Close </button>
    </div>
  </div>

</template>

<style scoped>

</style>