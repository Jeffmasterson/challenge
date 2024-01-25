<template>
  <div class="container mx-auto p-4 pt-10">
    <SearchBar
        :query="searchQuery"
        @update:query="onSearchQueryUpdate"
    />
    <DropDown
        :selectedCountry="selectedCountry"
        :selectedRegion="selectedRegion"
        :countryNames="countryName"
        :regionNames="regionName"
        @update:selectedCountry="updateSelectedHome"
        @update:selectedRegion="UpdateSelectedRegion"
    />
    <button class="inline-flex justify-center rounded-md bg-blue-500 hover:bg-blue-700 px-3 py-2 text-sm font-semibold text-white shadow-sm mt-0 lg:mt-3 ml-0 lg:ml-3 sm:w-auto" @click="clearSearch">Clear Search</button>
    <DataTable
        class="mt-10"
        :items="paginatedData"
    />
    <RegisterModal
        class="inline-flex"
        :formData="newRecord"
        @addRecord="addRecord"
    />
    <PageDropDown
        :perPage="itemsPerPage"
        @update:perPage="itemsPerPage = $event"
    />
    <paginate
        :page-count="totalPages"
        :click-handler="handlePageChange"
        :prev-text="'Prev'"
        :next-text="'Next'"
        :container-class="'pagination mt-5'"
        :force-page="currentPage"
    />
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import jsonData from '../data.json'
import Paginate from "vuejs-paginate-next"
import DropDown from "@/components/DropDown.vue"
import DataTable from "@/components/DataTable.vue"
import RegisterModal from "@/components/RegisterModal.vue"
import SearchBar from "@/components/SearchBar.vue";
import PageDropDown from "@/components/PageDropDown.vue"

const data = ref(jsonData)
const searchQuery = ref("")
const selectedCountry = ref(null)
const selectedRegion = ref(null)
const totalRecords = ref(0);
const currentPage = ref(1)
const itemsPerPage = ref(10)
const newRecord = ref({ name: '', email: '', country: '', region: '' })


const filteredData = computed(() => {
  const query = searchQuery.value ? searchQuery.value.toLowerCase() : ''
  return data.value
      .filter(item => !selectedCountry.value || item.country === selectedCountry.value)
      .filter(item => !selectedRegion.value || item.region === selectedRegion.value)
      .filter(item => {
        if (query) {
          pageReset()
          return item.name.toLowerCase().includes(query) || item.email.toLowerCase().includes(query)
        }
        return true
      })
})

const paginatedData = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage.value
  return filteredData.value
      .sort((a, b) => a.name.localeCompare(b.name))
      .slice(startIndex, startIndex + itemsPerPage.value)
})

const totalPages = computed(() => Math.ceil(totalRecords.value / itemsPerPage.value))

const uniqueValues = (key) => [...new Set(data.value.map(item => item[key]))].sort()

const countryName = computed(() => uniqueValues('country'))

const regionName = computed(() => uniqueValues('region'))

watch(filteredData, (newFilteredData) => {
  totalRecords.value = newFilteredData.length
})

const addRecord = record => {
  data.value.push({ ...record, id: Date.now() })
  totalRecords.value = data.value.length
  newRecord.value = { name: '', email: '', country: '', region: '' }
}

const handlePageChange = pageNumber => currentPage.value = pageNumber

const UpdateSelectedRegion = region => (selectedRegion.value = region, pageReset())

const updateSelectedHome = country => (selectedCountry.value = country, pageReset())

const onSearchQueryUpdate = newQuery => (searchQuery.value = newQuery, pageReset())

const clearSearch = () => {selectedRegion.value = selectedCountry.value = searchQuery.value = ''}

const pageReset = () => currentPage.value = 1

onMounted(() => {totalRecords.value = data.value.length})

</script>