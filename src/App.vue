<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref, watchEffect, computed } from 'vue'
import DemoGrid from './Grid.vue'
import DemoChart from './BarChart.vue'
const API_BASE = `/api/`
const API_ENDPOINTS = ['stocks', 'incomes', 'sales', 'orders']
const currentEndpoint = ref(API_ENDPOINTS[0])
const dateFrom = ref('2025-09-26')
const dateTo = ref('2025-09-26')
const page = ref('1')
const limit = ref('25')
const result = ref([])
const searchQuery = ref('')

const gridColumns = computed(() => {
  if (currentEndpoint.value === 'stocks') {
    return [
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
  }
  if (currentEndpoint.value === 'orders') {
    return [
      'g_number',
      'date',
      'last_change_date',
      'date',
      'supplier_article',
      'tech_size',
      'barcode',
      'total_price',
      'discount_percent',
      'warehouse_name',
      'oblast',
      'income_id',
      'odid',
      'nm_id',
      'subject',
      'category',
      'brand',
      'is_cancel',
      'cancel_dt',
    ]
  }
  if (currentEndpoint.value === 'sales') {
    return [
      'g_number',
      'date',
      'last_change_date',
      'supplier_article',
      'tech_size',
      'barcode',
      'total_price',
      'discount_percent',
      'is_supply',
      'is_realization',
      'promo_code_discount',
      'warehouse_name',
      'country_name',
      'oblast_okrug_name',
      'region_name',
      'income_id',
      'sale_id',
      'odid',
      'spp',
      'for_pay',
      'finished_price',
      'price_with_disc',
      'nm_id',
      'subject',
      'category',
      'brand',
      'is_storno',
    ]
  }
  if (currentEndpoint.value === 'incomes') {
    return [
      'income_id',
      'number',
      'date',
      'last_change_date',
      'supplier_article',
      'tech_size',
      'barcode',
      'quantity',
      'total_price',
      'date_close',
      'warehouse_name',
      'nm_id',
    ]
  }
})

function makeParams() {
  const API_PARAMS = `?dateFrom=${dateFrom.value}&dateTo=${dateTo.value}&page=${page.value}&key=E6kUTYrYwZq2tN4QEtyzsbEBk3ie&limit=${limit.value}`
  return API_PARAMS
}
const gridData = computed(() => {
  if (result.value.data && Array.isArray(result.value.data)) {
    if (currentEndpoint.value === 'stocks') {
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
    if (currentEndpoint.value === 'orders') {
      return result.value.data.map((item) => ({
        g_number: item.g_number || 'NULL',
        date: item.date || 'NULL',
        last_change_date: item.last_change_date || 'NULL',
        supplier_article: item.supplier_article || 'NULL',
        tech_size: item.tech_size || 'NULL',
        barcode: item.barcode || 'NULL',
        total_price: item.total_price || 'NULL',
        discount_percent: item.discount_percent || 'NULL',
        warehouse_name: item.warehouse_name || 'NULL',
        oblast: item.oblast || 'NULL',
        income_id: item.income_id || 'NULL',
        odid: item.odid || 'NULL',
        nm_id: item.nm_id || 'NULL',
        subject: item.subject || 'NULL',
        category: item.category || 'NULL',
        brand: item.brand || 'NULL',
        is_cancel: item.is_cancel || 'NULL',
        cancel_dt: item.cancel_dt || 'NULL',
      }))
    }
    if (currentEndpoint.value === 'sales') {
      return result.value.data.map((item) => ({
        g_number: item.g_number || 'NULL',
        date: item.date || 'NULL',
        last_change_date: item.last_change_date || 'NULL',
        supplier_article: item.supplier_article || 'NULL',
        tech_size: item.tech_size || 'NULL',
        barcode: item.barcode || 'NULL',
        total_price: item.total_price || 'NULL',
        discount_percent: item.discount_percent || 'NULL',
        is_supply: item.is_supply || 'NULL',
        is_realization: item.is_realization || 'NULL',
        promo_code_discount: item.promo_code_discount || 'NULL',
        warehouse_name: item.warehouse_name || 'NULL',
        country_name: item.country_name || 'NULL',
        oblast_okrug_name: item.oblast_okrug_name || 'NULL',
        region_name: item.region_name || 'NULL',
        income_id: item.income_id || 'NULL',
        sale_id: item.sale_id || 'NULL',
        odid: item.odid || 'NULL',
        spp: item.spp || 'NULL',
        for_pay: item.for_pay || 'NULL',
        finished_price: item.finished_price || 'NULL',
        price_with_disc: item.price_with_disc || 'NULL',
        nm_id: item.nm_id || 'NULL',
        subject: item.subject || 'NULL',
        category: item.category || 'NULL',
        brand: item.brand || 'NULL',
        is_storno: item.is_storno || 'NULL',
      }))
    }
    if (currentEndpoint.value === 'incomes') {
      return result.value.data.map((item) => ({
        income_id: item.income_id || 'NULL',
        number: item.number || 'NULL',
        date: item.date || 'NULL',
        last_change_date: item.last_change_date || 'NULL',
        supplier_article: item.supplier_article || 'NULL',
        tech_size: item.tech_size || 'NULL',
        barcode: item.barcode || 'NULL',
        quantity: item.quantity || 'NULL',
        total_price: item.total_price || 'NULL',
        date_close: item.date_close || 'NULL',
        warehouse_name: item.warehouse_name || 'NULL',
        nm_id: item.nm_id || 'NULL',
      }))
    }
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
  for (let key in gridData.value) {
    datecombs.push(gridData.value[key].last_change_date)
  }
  return datecombs
})
const dataprice = computed(() => {
  const dataprices = []
  for (let key in gridData.value) {
    dataprices.push(gridData.value[key].price)
  }
  return dataprices
})

watchEffect(async () => {
  const res = makeParams()
  const url = `${API_BASE}${currentEndpoint.value}${res}`
  result.value = await (await fetch(url)).json()
})

const chartData = computed(() => {
  const data = {}

  gridData.value.forEach((item) => {
    const warehouse_name = item.warehouse_name
    if (warehouse_name && warehouse_name !== 'NULL') {
      if (!data[warehouse_name]) {
        data[warehouse_name] = 0
      }
      if (currentEndpoint.value === 'stocks') {
        data[warehouse_name] += Number(item.price) || 0
      }
      if (currentEndpoint.value === 'orders') {
        data[warehouse_name] += Number(item.total_price) || 0
      }
      if (currentEndpoint.value === 'incomes') {
        data[warehouse_name] += Number(item.quantity) || 0
      }
      if (currentEndpoint.value === 'sales') {
        data[warehouse_name] += Number(item.total_price) || 0
      }
    }
  })
  const labels = Object.keys(data).sort()
  const GeneralData = labels.map((warehouse_name) => data[warehouse_name])
  return {
    labels: labels,
    datasets: [
      {
        label: `Bar - ${currentEndpoint.value}`,
        data: GeneralData,
        backgroundColor: 'rgba(54, 162, 235, 0.2)',
        borderColor: 'rgba(54, 162, 235, 1)',
        borderWidth: 1,
      },
    ],
  }
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
    <DemoChart :data="chartData"></DemoChart>
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
