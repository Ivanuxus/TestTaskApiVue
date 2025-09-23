<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref, watchEffect, computed } from 'vue'
import DemoGrid from './Grid.vue'
//    E6kUTYrYwZq2tN4QEtyzsbEBk3ie
const API_BASE = `http://109.73.206.144:6969/api/`
const API_ENDPOINTS = ['stocks', 'incomes', 'sales', 'orders']
const currentEndpoint = ref(API_ENDPOINTS[0])
const dateFrom = ref()
const dateTo = ref()
const page = ref()
const limit = ref()
const result = ref([])
const searchQuery = ref('')

const gridColumns = [
  'barcode',
  'brand',
  'category',
  'date',
  'discount',
  'in_way_from_client',
  'in_way_to_client',
  'is_realization',
  'is_supply',
  'last_change_date',
  'nm_id',
  'price',
  'quantity',
  'quantity_full',
  'subject',
  'supplier_article',
  'tech_size',
  'warehouse_name',
]
function makeParams() {
  // const API_PARAMS = `?dateFrom=${dateFrom.value}&dateTo=${dateTo.value}&page=${page.value}&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=${limit.value}`
  const API_PARAMS = `?dateFrom=2025-09-23&dateTo=2025-09-23&page=1&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=100`
  return API_PARAMS
}
const gridData = computed(() => {
  if (result.value.data && Array.isArray(result.value.data)) {
    return result.value.data.map((item) => ({
      barcode: item.barcode || 'NULL',
      brand: item.brand || 'NULL',
      category: item.category || 'NULL',
      date: item.date || 'NULL',
      discount: item.discount || 'NULL',
      in_way_from_client: item.in_way_from_client || 'NULL',
      in_way_to_client: item.in_way_to_client || 'NULL',
      is_realization: item.is_realization || 'NULL',
      is_supply: item.is_supply || 'NULL',
      last_change_date: item.last_change_date || 'NULL',
      nm_id: item.nm_id || 'NULL',
      price: item.price || 'NULL',
      quantity: item.quantity || 'NULL',
      quantity_full: item.quantity_full || 'NULL',
      subject: item.subject || 'NULL',
      supplier_article: item.supplier_article || 'NULL',
      tech_size: item.tech_size || 'NULL',
      warehouse_name: item.warehouse_name || 'NULL',
    }))
  }
  return []
})

const warehouse = computed(() => {
  const warehouses = []
  for (let key in gridData.value) {
    warehouses.push(gridData.value[key].warehouse_name)
  }
  return warehouses
})

const datecomb = computed(() => {
  const datecombs = []
  console.log(gridData.value)
  for (let key in gridData.value) {
    datecombs.push(gridData.value[key].last_change_date)
  }

  return datecombs
})

watchEffect(async () => {
  const res = makeParams()
  const url = `${API_BASE}${currentEndpoint.value}${res}`
  result.value = await (await fetch(url)).json()
})
</script>
<template>
  <div style="">
    <template v-for="Endpoint in API_ENDPOINTS">
      <input
        type="radio"
        :id="Endpoint"
        :value="Endpoint"
        v-on:change="makeParams"
        name="Endpoint"
        style="top: 0px"
        v-model="currentEndpoint"
      />
      <label :for="Endpoint" :style="{ position: 'relative' }">{{ Endpoint }}<br /></label>
    </template>
    <h5 class="text">DateFrom</h5>
    <input v-model="dateFrom" />
    <h5 class="text">dateTo</h5>
    <input v-model="dateTo" />
    <h5 class="text">Page</h5>
    <input v-model="page" />
    <h5 class="text">Limit of symbols</h5>
    <input v-model="limit" />
    <form id="search" style="display: flex">
      Search <input name="query" v-model="searchQuery" style="display: inline-block" />
    </form>
    <select v-model="searchQuery" style="position: relative; display: inline-block">
      <option value="">Empty</option>
      <option v-for="itemWare in warehouse" :filter-key="itemWare">{{ itemWare }}</option>
    </select>
    <select v-model="searchQuery" style="position: relative; display: inline-block; width: 150px">
      <option value="">Empty</option>
      <option v-for="itemDateCh in datecomb" :filter-key="itemDateCh">{{ itemDateCh }}</option>
    </select>
    <DemoGrid :data="gridData" :columns="gridColumns" :filter-key="searchQuery"> </DemoGrid>
  </div>
</template>
<style>
@import 'https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css';
.text {
  display: inline-block;
  margin: 10px;
}

.pagination {
  display: flex;
  justify-content: center;
  gap: 5px;
}
.page-item {
  padding: 10px 15px;
  border: 1px solid #ddd;
  cursor: pointer;
}
.page-item:hover {
  background-color: #f0f0f0;
}
.active-page {
  background-color: #007bff;
  color: #fff;
}
</style>
