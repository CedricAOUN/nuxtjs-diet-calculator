<script setup lang="ts">

const { title, quantity, foodId } = defineProps(['title', 'quantity', 'foodId'])

const itemList = useState<any>('itemList')
function handleRemove() {
  itemList.value.map((item: any, index: number) => {
    if(item.id == foodId) {
      itemList.value.splice(index, 1)
    }
  })
}

</script>

<template>
  <div class="flex gap-5 justify-between items-center border-2 dark:border-gray-300 border-gray-900 rounded p-4" >
    <div>
      <h1 class="text-lg">{{ title.toString().charAt(0).toUpperCase() + title.toString().slice(1) }}</h1>
      <p class="text-sm">Quantity: <span class="text-green-400">{{ quantity }}</span></p>
    </div>
    <button @click="handleRemove" class="inline-flex items-center px-3 py-2 font-medium text-center text-white bg-red-400 rounded-lg hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:bg-red-600 dark:hover:bg-red-400 dark:focus:ring-red-300">
      Remove
    </button>
  </div>
</template>

<style scoped>

</style>