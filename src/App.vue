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
  const API_PARAMS = `?dateFrom=2025-09-22&dateTo=2025-09-22&page=1&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=100`
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
const resultBoxWare = []
const selected = ref('')
const warehouse = []
const datecomb = []
function fillBox() {
  for (let key in gridData) {
    if (gridData[key]) {
      resultBoxWare.push(gridData[key])
    }
  }
  const resultBoxWare1 = resultBoxWare[1]
  for (let key in resultBoxWare1) {
    warehouse.push(resultBoxWare1[key].warehouse_name)
    datecomb.push(resultBoxWare1[key].date)
  }
  console.log(warehouse)
  searchQuery = selected
}
watchEffect(async () => {
  const res = makeParams()
  const url = `${API_BASE}${currentEndpoint.value}${res}`
  result.value = await (await fetch(url)).json()
  console.log(result.value)
})
</script>
<template>
  <div style="position: absolute; left: 10px; top: 10px">
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
      <label :for="Endpoint" :style="{ position: 'relative', paddingTop: '10px' }"
        >{{ Endpoint }}<br
      /></label>
    </template>
    <h2 class="text">DateFrom</h2>
    <input v-model="dateFrom" />
    <h2 class="text">dateTo</h2>
    <input v-model="dateTo" />
    <h2 class="text">Page</h2>
    <input v-model="page" />
    <h2 class="text">Limit of symbols</h2>
    <input v-model="limit" />
    <button
      @click="fillBox"
      style="position: absolute; left: +100px; top: +50px; width: 75px; height: 50px"
    >
      Fill
    </button>
    <select v-model="selected" style="display: flex">
      <option v-for="item in warehouse">{{ item }}</option>
    </select>
  </div>
  <div style="position: absolute; left: 250px">
    <form id="search">Search <input name="query" v-model="searchQuery" /></form>
    <DemoGrid :data="gridData" :columns="gridColumns" :filter-key="searchQuery"> </DemoGrid>
  </div>
</template>
<style>
.text {
  position: relative;
  padding-top: 10px;
}
</style>
