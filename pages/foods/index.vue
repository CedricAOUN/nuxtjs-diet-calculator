<script setup lang="ts">

  const inputValue = ref('');
  const isLoading = ref(false);

  // async function searchItems(query: string) {
  //   const url = `https://api.spoonacular.com/food/search?query=${query}&number=10&apiKey=dac835290173412eaf8ca3f8a4011a1b`
  //   if(inputValue.value != '') {
  //     foodItems.value = [];
  //     isLoading.value = true;
  //     inputValue.value = '';
  //     try {
  //       const {data: newSearch} = await useFetch<any>(url);
  //
  //       newSearch.value.searchResults.map((result: any, index: number) => {
  //         if (result.name.toLowerCase() == 'menu items') {
  //           result.results.map((res: any) => res.foodType = 'menu')
  //           foodItems.value = [...foodItems.value, ...result.results]
  //         }
  //         if (result.name.toLowerCase() == 'simple foods') {
  //           result.results.map((res: any) => res.foodType = 'ingredient')
  //           foodItems.value = [...foodItems.value, ...result.results]
  //         }
  //       })
  //       isLoading.value = false;
  //
  //
  //     } catch (e) {
  //       console.log(e);
  //     }
  //   }
  // }

  let foodItems = ref<any[] | undefined>(undefined);

  interface SearchResponse  {
    text: string,
    parsed: any[],
    hints: any[]
  }

  async function searchItems(query: string, brand?: string) {
    const url = `https://api.edamam.com/api/food-database/v2/parser?app_id=c7d1b491&app_key=f60db3c5476f4f0c68beb07c74ab65d1&nutrition-type=cooking&ingr=${query}&${brand && `brand=${brand}`}`
    if(query != '') {
      try {
        const {data: searchResults} = await useFetch<SearchResponse>(url)

        foodItems.value = searchResults.value?.hints


      } catch (e) {
        console.log('error while fetching search results:', e)
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
      <div v-if='foodItems != null' id="brands">
        <h2>Results:</h2>
        <div class="flex flex-row flex-wrap">
          <FoodCard v-for="i of foodItems" :food-item="i"></FoodCard>
        </div>
      </div>
    <div v-if="isLoading">
      <Icon name="charm:refresh" class="text-green-400 animate-spin" size="3em">

      </Icon>
    </div>
  </section>

</template>

<style scoped>

</style>