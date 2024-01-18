<template>
  <div class="inline-flex">
    <div class="select-container">
      <select v-model="internalPerPage" @change="onPerPageChange" class="inline-flex w-auto justify-center rounded-md bg-blue-500 hover:bg-blue-700 px-3 py-2 text-sm font-semibold text-white shadow-sm ml-3">
        <option v-for="option in options" :key="option" :value="option">
          {{ option }}
        </option>
      </select>
      Items Per Page
    </div>
  </div>
</template>

<script>
export default {
  props: {
    perPage: {
      type: Number,
      required: true
    },
    options: {
      type: Array,
      default: () => [5, 10, 15, 20]
    }
  },
  data() {
    return {
      internalPerPage: this.perPage
    };
  },
  methods: {
    onPerPageChange() {
      this.$emit('update:perPage', this.internalPerPage);
    }
  },
  watch: {
    perPage(newVal) {
      this.internalPerPage = newVal;
    }
  }
};
</script>