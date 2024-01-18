<template>
  <div class="flex border-2 rounded w-full lg:inline-flex lg:w-80">
    <input
        type="text"
        class="px-4 py-2 w-full"
        :placeholder="placeholder"
        v-model="internalQuery"
        @input="$emit('update:query', internalQuery)"
    >
  </div>
</template>

<script>
export default {
  props: {
    query: {
      type: String,
      required: true
    },
    placeholder: {
      type: String,
      default: 'Search for name or email...'
    }
  },
  data() {
    return {
      internalQuery: this.query
    };
  },
  watch: {
    query(newVal) {
      this.internalQuery = newVal;
    }
  }
};
</script>