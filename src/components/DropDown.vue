<template>
  <div class="flex w-full lg:inline-flex lg:w-auto">
    <div class="dropdown mt-3 ml-0 mb-3 lg:mt-3 lg:ml-3">
      <select v-model="localSelectedHome" @change="onCountryChange" class="inline-flex w-full justify-center rounded-md bg-blue-500 hover:bg-blue-700 px-3 py-2 text-sm font-semibold text-white shadow-sm sm:w-auto">
        <option value="" disabled>Select Country</option>
        <option v-for="country in countryNames" :key="country" :value="country">
          {{ country }}
        </option>
      </select>
    </div>
    <div class="dropdown mt-3 ml-3 mb-3 lg:mt-3 lg:ml-3">
      <select v-model="localSelectedRegion" @change="onRegionChange" class="inline-flex w-full justify-center rounded-md bg-blue-500 hover:bg-blue-700 px-3 py-2 text-sm font-semibold text-white shadow-sm sm:w-auto">
        <option value="" disabled>Select Region</option>
        <option v-for="region in regionNames" :key="region" :value="region">
          {{ region }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import { ref, watch, defineComponent } from 'vue';

export default defineComponent({
  props: {
    selectedCountry: String,
    selectedRegion: String,
    countryNames: Array,
    regionNames: Array,
  },
  setup(props, { emit }) {
    const localSelectedHome = ref(props.selectedCountry || '');
    const localSelectedRegion = ref(props.selectedRegion || '');

    const onCountryChange = () => {
      emit('update:selectedCountry', localSelectedHome.value);
    };

    const onRegionChange = () => {
      emit('update:selectedRegion', localSelectedRegion.value);
    };

    watch(() => props.selectedCountry, (newVal) => {
      localSelectedHome.value = newVal;
    });

    watch(() => props.selectedRegion, (newVal) => {
      localSelectedRegion.value = newVal;
    });

    return {
      localSelectedHome,
      localSelectedRegion,
      onCountryChange,
      onRegionChange
    };
  },
});
</script>

<style>
/* Add any additional CSS styling here */
</style>
