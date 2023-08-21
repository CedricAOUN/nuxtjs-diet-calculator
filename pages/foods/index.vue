<script setup lang="ts">

  const defaultUrl: string = "https://api.spoonacular.com/food/menuItems/search?query=burger&number=10&apiKey=dac835290173412eaf8ca3f8a4011a1b"

  const {data: defaultSearch} = await useFetch<any>(defaultUrl)

  let menuItems = ref(defaultSearch.value.menuItems);

  const inputValue = ref('');

  const isLoading = ref(false);

  async function searchItems(query: string) {
    isLoading.value = true;
    const url = `https://api.spoonacular.com/food/menuItems/search?query=${query}&number=10&apiKey=dac835290173412eaf8ca3f8a4011a1b`
    try {
      const {data: newSearch } = await useFetch<any>(url);

      if(newSearch.value.menuItems.length > 0) {
        isLoading.value = false;
        return menuItems.value = newSearch.value.menuItems
      } else {
        isLoading.value = false;
        return menuItems.value = [];

      }

    } catch(e) {
      console.log(e);
    }

  }

</script>

<template>
  <section class="text-center py-16">
    <h1 class="text-4xl">Foods:</h1>
      <input v-model="inputValue" class="p-4 m-2 bg-neutral-300 mx-auto block max-w-[800px] w-full" placeholder="Enter a search term, like a fast food menu item, or any type of food!">
      <button class="m-2 p-2 rounded-lg bg-green-400 text-white" @click="searchItems(inputValue)">
        Search
      </button>
  </section>
  <section class="text-center py-16 h-full flex justify-center gap-4 flex-wrap">
      <template v-if="menuItems.length > 0 && !isLoading" v-for="item in menuItems" :key="item.id">
        <FoodCard :food-item="item"/>
      </template>
      <p v-if="menuItems.length == 0" class="text-center">No search results found! Try a different search term.</p>
    <div v-if="isLoading">
      <Icon name="charm:refresh" class="text-green-400 animate-spin" size="3em">

      </Icon>
    </div>
  </section>



</template>

<style scoped>

</style>