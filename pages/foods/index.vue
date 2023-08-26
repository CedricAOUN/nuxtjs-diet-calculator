<script setup lang="ts">


  const inputValue = ref('');
  const isLoading = ref(false);


  let foodItems = ref<any[] | undefined>(undefined);


  interface SearchResponse  {
    text: string,
    parsed: any[],
    hints: any[]
  }

  function handleChange(toQuery: string) {
    inputValue.value = toQuery
    searchItems(inputValue.value)
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
  const autoList = ref<string[] | null>([])
  const autoComplete = async (query: string) => {
    const url = `https://api.edamam.com/auto-complete?app_id=c7d1b491&app_key=f60db3c5476f4f0c68beb07c74ab65d1&q=${query}`
    try {
      const {data: resList } = await useFetch<string[]>(url)
      if(resList.value) autoList.value = resList.value

    } catch (e) {
      console.log('Error fetching auto completion: ', e)
    }
  }


  watch(inputValue, (newQuery: string) => {
    autoComplete(newQuery)
  })

</script>

<template>
  <section class="text-center py-16 mx-2 md:mx-0">
    <h1 class="text-4xl mb-4">Foods:</h1>
    <div class="grid grid-cols-4 gap-5 space-y-2 mx-auto w-full relative max-w-[800px] ">
      <input id='search' v-model="inputValue" autocomplete="off"
             class="p-4 col-span-4 sm:col-span-3 bg-neutral-300 dark:bg-neutral-600 w-full max-w-[800px] "
              placeholder="Enter a brand, or a specific type of food/ingredient">
      <button class="p-2 sm:col-start-4 col-span-2 col-start-2 row-start-1 h-12 my-auto rounded-lg bg-green-400 text-white" @click="searchItems(inputValue)">
        Search
      </button>
      <ul v-if="autoList?.length > 0" id="auto-complete-list" class="text-left sm:col-start-1 sm:col-end-4 w-full row-start-3 sm:row-start-2 text-black absolute bg-white dark:bg-gray-600 dark:text-white border border-gray-300 rounded shadow-md ">
        <li v-for="s in autoList" class="hover:bg-gray-400 bg-opacity-50 cursor-pointer hover:text-white px-3 py-3" @click="handleChange(s)">{{s.charAt(0).toUpperCase() + s.slice(1)}}</li>
      </ul>
    </div>

    <div id="edamam-badge" data-color="white"></div>
  </section>
  <section class="text-center h-full flex justify-center gap-4 flex-col px-5 flex-wrap">
    <h2 class="text-4xl">Results:</h2>
      <div v-if='foodItems?.length > 0'>
        <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 justify-center gap-2 ">
          <FoodCard class="mx-auto w-full" v-for="i of foodItems" :food-item="i"></FoodCard>
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