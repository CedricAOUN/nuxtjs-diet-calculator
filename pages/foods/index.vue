<script setup lang="ts">


  let foodItems = ref<any[]>([]);

  const inputValue = ref('');

  const isLoading = ref(false);

  async function searchItems(query: string) {
    const url = `https://api.spoonacular.com/food/search?query=${query}&number=10&apiKey=dac835290173412eaf8ca3f8a4011a1b`
    if(inputValue.value != '') {
      foodItems.value = [];
      isLoading.value = true;
      inputValue.value = '';
      try {
        const {data: newSearch} = await useFetch<any>(url);

        newSearch.value.searchResults.map((result: any, index: number) => {
          if (result.name.toLowerCase() == 'menu items') {
            result.results.map((res: any) => res.foodType = 'menu')
            foodItems.value = [...foodItems.value, ...result.results]
          }
          if (result.name.toLowerCase() == 'simple foods') {
            result.results.map((res: any) => res.foodType = 'ingredient')
            foodItems.value = [...foodItems.value, ...result.results]
          }
        })
        isLoading.value = false;


      } catch (e) {
        console.log(e);
      }
    }
  }

</script>

<template>
  <section class="text-center py-16">
    <h1 class="text-4xl">Foods:</h1>
      <input id='search' v-model="inputValue" class="p-4 m-2 bg-neutral-300 mx-auto block max-w-[800px] w-full" placeholder="Enter a search term, like a fast food menu item, or any type of food!">
      <button class="m-2 p-2 rounded-lg bg-green-400 text-white" @click="searchItems(inputValue)">
        Search
      </button>
  </section>
  <section class="text-center py-16 h-full flex justify-center gap-4 flex-wrap">
      <template v-if="foodItems.length > 0 && !isLoading" v-for="item in foodItems" :key="item.id">
        <FoodCard :food-item="item" :type="item.foodType"/>
      </template>
      <p v-if="foodItems.length == 0" class="text-center">No search results found! Try a different search term.</p>
    <div v-if="isLoading">
      <Icon name="charm:refresh" class="text-green-400 animate-spin" size="3em">

      </Icon>
    </div>
  </section>



</template>

<style scoped>

</style>