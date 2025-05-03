<script setup>
import { ref, computed } from 'vue';

const items = ref([])
const newItem = ref('')
const filter = ref('all')

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

const filteredItems = computed(() => {
  if (filter.value === 'all') {
    return items.value
  } else if (filter.value === 'completed') {
    return items.value.filter(item => item.completed)
  } else if (filter.value === 'uncompleted') {
    return items.value.filter(item => !item.completed)
  }
  return items.value
})

</script>

<template>
  <div class="min-h-screen w-screen bg-gray-200 text-gray-800 flex items-center justify-center p-4 overflow-hidden">
    <div class="bg-white shadow-2xl rounded-xl p-6 w-full max-w-xl h-[90vh] flex flex-col gap-4">
      
      <h2 class="text-2xl font-bold text-center text-blue-600 mb-2">My Daily List</h2>

      <!-- Input -->
      <div class="flex gap-2">
        <input
          v-model="newItem"
          @keyup.enter="addItem"
          type="text"
          placeholder="Tambahkan item..."
          class="flex-1 p-3 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition"
        />
        <button
          @click="addItem"
          class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition shadow"
        >
          Tambah
        </button>
      </div>

      <!-- Filter -->
      <div class="mt-2">
        <select
          v-model="filter"
          class="w-full p-2 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400"
        >
          <option value="all">Semua</option>
          <option value="completed">Selesai</option>
          <option value="uncompleted">Belum Selesai</option>
        </select>
      </div>

      <!-- Daftar Item -->
      <ul class="overflow-y-auto flex-1 mt-2 space-y-3 custom-scroll pr-2">
        <li
          v-for="item in filteredItems"
          :key="item.id"
          class="flex items-center justify-between bg-gray-100 hover:bg-gray-200 p-3 rounded-md shadow-sm transition-transform hover:scale-[1.01]"
        >
          <div class="flex items-center gap-3">
            <input
              type="checkbox"
              v-model="item.completed"
              @change="toggleCompleted(item)"
              class="w-5 h-5 text-blue-500"
            />
            <span :class="{ 'line-through text-gray-500': item.completed }">{{ item.text }}</span>
          </div>
          <button @click="removeItem(item)" class="text-red-500 hover:text-red-700 transition">✕</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
/* Cegah scroll window */
html, body {
  overflow: hidden;
}

/* Custom scrollbar untuk daftar item */
.custom-scroll::-webkit-scrollbar {
  width: 6px;
}
.custom-scroll::-webkit-scrollbar-thumb {
  background-color: rgba(107, 114, 128, 0.5); /* gray-500 */
  border-radius: 4px;
}
.custom-scroll {
  scrollbar-width: thin;
  scrollbar-color: rgba(107, 114, 128, 0.5) transparent;
}
</style>
