<script setup lang="ts">
import { ref } from 'vue';

const darkMode = ref(false);

  function handleToggle() {
    darkMode.value = !darkMode.value;
  }

</script>

<template>

  <header :class="{dark: darkMode}">
    <nav class="px-[30rem] shadow-sm shadow-black dark:bg-gray-800 dark:text-white">
      <ul class="flex flex-row justify-between gap-5 h-[75px] items-center">
        <li class="mr-auto">
          <NuxtLink to="/"><div class="flex flex-row gap-5">
            <img class='h-6'
                                   src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/Circle-icons-calculator.svg/1200px-Circle-icons-calculator.svg.png"
                                   alt="site calculator logo"/>
            <p class="">Diet Calculator</p>
          </div> </NuxtLink>
        </li>
        <li class="">
          <NuxtLink class='p-2 rounded-lg' to="/">Home</NuxtLink></li>
        <li>
          <NuxtLink class='p-2 rounded-lg' to="/foods">Foods</NuxtLink>
        </li>
        <li>
          <NuxtLink class='p-2 rounded-lg' to="/results">Result</NuxtLink>
        </li>
        <li>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" value="" class="sr-only peer" v-on:click="handleToggle">
            <span class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></span>
            <span class="ml-3 text-sm font-medium text-gray-900 dark:text-gray-300">{{darkMode ? 'Light' : 'Dark'}} mode</span>
          </label>
        </li>
      </ul>

    </nav>
  </header>

  <div class="" v-bind:class="{dark: darkMode}">
    <div class="dark:bg-gray-600 dark:text-white">
      <NuxtPage></NuxtPage>
    </div>
  </div>
</template>

<style scoped>
.router-link-active {
  @apply bg-green-400 text-white
}
body {
 @apply bg-red-700
}

</style>