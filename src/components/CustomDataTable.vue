<script setup>
import Paginate from "vuejs-paginate-next";
import DropDown from "@/components/DropDown.vue";
import DataTable from "@/components/DataTable.vue";
import RegisterModal from "@/components/RegisterModal.vue";
import SearchBar from "@/components/SearchBar.vue";
import PageDropDown from "@/components/PageDropDown.vue";

</script>

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

<script>
import jsonData from '../data.json';

export default {
  name: 'DataTable',
  components: {
    SearchBar,
    DropDown,
    DataTable,
    RegisterModal,
    PageDropDown
  },
  data() {
    return {
      data: jsonData,
      searchQuery: "",
      selectedCountry: null,
      selectedRegion: null,
      totalRecords: 0,
      currentPage: 1,
      itemsPerPage: 10,
      pagesShown: 1,
      newRecord: { name: '', email: '', country: '', region: '' },
    };
  },
  computed: {
    filteredData() {
      let filtered = this.data;

      if (this.selectedCountry) {
        filtered = filtered.filter(item => item.country === this.selectedCountry);
      }

      if (this.selectedRegion) {
        filtered = filtered.filter(item => item.region === this.selectedRegion);
      }

      if (this.searchQuery) {
        this.currentPage = 1;
        const query = this.searchQuery.toLowerCase();
        filtered = filtered.filter(item =>
            item.name.toLowerCase().includes(query) || item.email.toLowerCase().includes(query)
        );
      }

      return filtered;
    },

    paginatedData() {
      let sortedAndFiltered = this.filteredData;

      sortedAndFiltered.sort((a, b) => a.name.localeCompare(b.name));

      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return sortedAndFiltered.slice(startIndex, endIndex);
    },

    totalPages() {
      return Math.ceil(this.totalRecords / this.itemsPerPage);
    },

    countryName() {
      const uniqueCountry = new Set();
      this.data.forEach(item => {
        uniqueCountry.add(item.country);
      });
      return Array.from(uniqueCountry).sort();
    },

    regionName() {
      const uniqueRegion = new Set();
      this.data.forEach(item => {
        uniqueRegion.add(item.region);
      });
      return Array.from(uniqueRegion).sort();
    }
  },
  watch: {
    filteredData(newFilteredData) {
      this.totalRecords = newFilteredData.length;
    },
  },
  methods: {
    addRecord(record) {
      this.data.push({ ...record, id: Date.now() });
      this.totalRecords = this.data.length;
      this.newRecord = { name: '', email: '', country: '', region: '' };
    },
    handlePageChange(pageNumber) {
      this.currentPage = pageNumber;
    },
    UpdateSelectedRegion(region) {
      this.selectedRegion = region;
      this.currentPage = 1;
    },
    updateSelectedHome(country) {
      this.selectedCountry = country;
      this.currentPage = 1;
    },

    onSearchQueryUpdate(newQuery) {
      this.searchQuery = newQuery;
      this.currentPage = 1;
    },

    clearSearch() {
      this.selectedRegion = '';
      this.selectedCountry = '';
      this.searchQuery = '';
    },
  },
  mounted() {
    this.totalRecords = this.data.length;
  },
};
</script>
