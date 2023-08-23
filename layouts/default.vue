<script setup lang="ts">
import { ref } from 'vue';

const darkMode = ref(false);

const openMenu = ref(false);

  function handleToggle() {
    darkMode.value = !darkMode.value;
  }

  function handleOpen() {
    openMenu.value = !openMenu.value
  }

</script>

<template>
  <header :class="{dark: darkMode}">
    <nav class="bg-white border-gray-200 dark:bg-gray-900">
      <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
        <button @click="handleOpen" type="button" class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600">
          <span class="sr-only">Open main menu</span>
          <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 17 14">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 1h15M1 7h15M1 13h15"/>
          </svg>
        </button>
        <div :class="`${!openMenu && 'hidden'} w-full md:block md:w-auto`" id="navbar-default`">
          <ul class="font-medium flex flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">
            <li class="mr-[25rem] flex gap-2">
              <img class="h-6 w-6"
                                                   src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/Circle-icons-calculator.svg/1024px-Circle-icons-calculator.svg.png"
                                                   alt="diet calculator logo">
              <p>Diet Calculator</p></li>
            <li>
              <NuxtLink to="/" class="block py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent" aria-current="page">Home</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/foods" class="block py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Foods</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/results"  class="block py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Results</NuxtLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>

  </header>

  <div class="" v-bind:class="{dark: darkMode}">
    <div class="dark:bg-gray-600 dark:text-white min-w-screen min-h-screen">
      <NuxtPage></NuxtPage>
    </div>
  </div>

</template>

<style scoped>
.router-link-active {
  @apply text-green-600
}


</style>