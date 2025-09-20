<script setup>
import { computed } from 'vue'

const props = defineProps({
  data: {
    type: Array,
    default: () => [], // Добавляем значение по умолчанию
  },
  columns: {
    type: Array,
    default: () => [], // Добавляем значение по умолчанию
  },
  filterKey: {
    type: String,
    default: '', // Добавляем значение по умолчанию
  },
})

const filteredData = computed(() => {
  const filterKey = props.filterKey && props.filterKey.toLowerCase()
  if (!filterKey) {
    return props.data || [] // Защита от undefined
  }
  return (props.data || []).filter((row) => {
    // Защита от undefined
    return Object.values(row).some((value) => {
      return String(value).toLowerCase().includes(filterKey)
    })
  })
})
</script>

<template>
  <table v-if="filteredData.length">
    <thead>
      <tr>
        <th v-for="key in columns" :key="key">
          {{ key }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(entry, index) in filteredData" :key="index">
        <td v-for="key in columns" :key="key">
          {{ entry[key] }}
        </td>
      </tr>
    </tbody>
  </table>
  <p v-else>No matching records found.</p>
</template>

<style>
table {
  border: 2px solid #42b883;
  border-radius: 3px;
  background-color: #fff;
  margin-top: 20px;
  width: 100%;
}

th {
  background-color: #42b883;
  color: rgba(255, 255, 255, 0.66);
  cursor: pointer;
  user-select: none;
  padding: 10px;
  text-align: left;
}

td {
  background-color: #f9f9f9;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

th,
td {
  min-width: 120px;
}

tr:hover td {
  background-color: #f0f0f0;
}
</style>
