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
import { ref, computed, watch, onMounted } from 'vue';
import jsonData from '../data.json';
import Paginate from "vuejs-paginate-next";
import DropDown from "@/components/DropDown.vue";
import DataTable from "@/components/DataTable.vue";
import RegisterModal from "@/components/RegisterModal.vue";
import SearchBar from "@/components/SearchBar.vue";
import PageDropDown from "@/components/PageDropDown.vue";

// Declare reactive data for the component
const data = ref(jsonData);
const searchQuery = ref("");
const selectedCountry = ref(null);
const selectedRegion = ref(null);
const totalRecords = ref(0);
const currentPage = ref(1);
const itemsPerPage = ref(10);
const newRecord = ref({ name: '', email: '', country: '', region: '' });

// Computed property for filtering data based on search criteria
const filteredData = computed(() => {
  let filtered = data.value;

  // Filter by country
  if (selectedCountry.value) {
    filtered = filtered.filter(item => item.country === selectedCountry.value);
  }

  // Filter by region
  if (selectedRegion.value) {
    filtered = filtered.filter(item => item.region === selectedRegion.value);
  }

  // Filter by search query
  if (searchQuery.value) {
    currentPage.value = 1;
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(item =>
        item.name.toLowerCase().includes(query) || item.email.toLowerCase().includes(query)
    );
  }

  return filtered;
});

// Computed property to calculate paginated data
const paginatedData = computed(() => {
  let sortedAndFiltered = filteredData.value;
  sortedAndFiltered.sort((a, b) => a.name.localeCompare(b.name));
  const startIndex = (currentPage.value - 1) * itemsPerPage.value;
  const endIndex = startIndex + itemsPerPage.value;
  return sortedAndFiltered.slice(startIndex, endIndex);
});

// Computed property to calculate total pages
const totalPages = computed(() => {
  return Math.ceil(totalRecords.value / itemsPerPage.value);
});

// Computed properties for country and region names
const countryName = computed(() => {
  const uniqueCountry = new Set();
  data.value.forEach(item => {
    uniqueCountry.add(item.country);
  });
  return Array.from(uniqueCountry).sort();
});

const regionName = computed(() => {
  const uniqueRegion = new Set();
  data.value.forEach(item => {
    uniqueRegion.add(item.region);
  });
  return Array.from(uniqueRegion).sort();
});

// Watcher to update totalRecords when filteredData changes
watch(filteredData, (newFilteredData) => {
  totalRecords.value = newFilteredData.length;
});

// Function to add a new record
function addRecord(record) {
  data.value.push({ ...record, id: Date.now() });
  totalRecords.value = data.value.length;
  newRecord.value = { name: '', email: '', country: '', region: '' };
}

// Function to handle page changes
function handlePageChange(pageNumber) {
  currentPage.value = pageNumber;
}

// Function to update selected region
function UpdateSelectedRegion(region) {
  selectedRegion.value = region;
  currentPage.value = 1;
}

// Function to update selected country
function updateSelectedHome(country) {
  selectedCountry.value = country;
  currentPage.value = 1;
}

// Function to update search query
function onSearchQueryUpdate(newQuery) {
  searchQuery.value = newQuery;
  currentPage.value = 1;
}

// Function to clear search criteria
function clearSearch() {
  selectedRegion.value = '';
  selectedCountry.value = '';
  searchQuery.value = '';
}

// Lifecycle hook to set initial total records count
onMounted(() => {
  totalRecords.value = data.value.length;
});
</script>


