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
function makeParams() {
  // const API_PARAMS = `?dateFrom=${dateFrom.value}&dateTo=${dateTo.value}&page=${page.value}&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=${limit.value}`
  const API_PARAMS = `?dateFrom=2025-09-20&dateTo=2025-09-20&page=1&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=100`
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
  </div>
  <DemoGrid :data="gridData"></DemoGrid>
</template>
<style>
.text {
  position: relative;
  padding-top: 10px;
}
</style>
