<script setup>
import { ref } from 'vue';

const items = ref([])
const newItem = ref('')

const addItem = () => {
  if (newItem.value.trim() !== '') {
    items.value.push({
      id: Date.now(),
      text: newItem.value,
      completed: false,
    })
    newItem.value = ''
  }
}

const removeItem = (item) => {
  items.value = items.value.filter(i => i.id !== item.id)
}

const toggleCompleted = (item) => {
  item.completed == !item.completed
}

</script>

<template>
  <div>
    <input type="text" v-model="newItem" @keyup.enter="addItem" />
    <button @click="addItem">Tambahkan</button>

    <ul>
      <li v-for="item in items" :key="item.id">
        <input type="checkbox" v-model="item.completed" @change="toggleCompleted(item)" />
        {{ item.text }}
        <button @click="removeItem(item)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
