<script setup lang="ts">

const { title, quantity } = defineProps(['title', 'quantity'])

const itemList = useState<any>('itemList')
function handleRemove() {
  itemList.value.map((item: any, index: number) => {
    if(item.name == title) {
      itemList.value.splice(index, 1)
    }
  })
}

</script>

<template>
  <div class="flex flex-col gap-5 items-center bg-green-400 dark:bg-green-600 rounded p-4" >
    <h1>{{title}}</h1>
    <p>Quantity: {{quantity}}</p>
    <button @click="handleRemove">
      Remove
    </button>
  </div>
</template>

<style scoped>

</style>