<script setup lang="ts">

  const inputValue = ref('');
  const isLoading = ref(false);


  let foodItems = ref<any[] | undefined>(undefined);


  interface SearchResponse  {
    text: string,
    parsed: any[],
    hints: any[]
  }

  async function searchItems(query: string, brand?: string) {
    foodItems.value = []
    isLoading.value = true;
    inputValue.value = '';
    const url = `https://api.edamam.com/api/food-database/v2/parser?app_id=c7d1b491&app_key=f60db3c5476f4f0c68beb07c74ab65d1&nutrition-type=cooking&ingr=${query}&${brand && `brand=${brand}`}`
    if(query != '') {
      try {
        const {data: searchResults} = await useFetch<SearchResponse>(url)
        foodItems.value = searchResults.value?.hints

        isLoading.value = false;

      } catch (e) {
        console.log('error while fetching search results:', e)
      }
    }
  }

</script>

<template>
  <section class="text-center py-16">
    <h1 class="text-4xl">Foods:</h1>
      <input id='search' v-model="inputValue" class="p-4 m-2 bg-neutral-300 dark:bg-neutral-600 mx-auto block max-w-[800px] w-full" placeholder="Enter a brand, or a specific type of food/ingredient">
      <button class="p-2 rounded-lg bg-green-400 text-white" @click="searchItems(inputValue)">
        Search
      </button>
  </section>
  <section class="text-center h-full flex justify-center gap-4 flex-col px-5 flex-wrap">
    <h2 class="text-4xl">Results:</h2>
      <div v-if='foodItems?.length > 0'>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 justify-center gap-2">
          <FoodCard v-for="i of foodItems" :food-item="i"></FoodCard>
        </div>
      </div>
      <p v-if="foodItems?.length == 0 && !isLoading">No Results Found</p>
    <div v-if="isLoading">
      <Icon name="charm:refresh" class="text-green-400 animate-spin" size="3em">

      </Icon>
    </div>
  </section>

</template>

<style scoped>

</style>