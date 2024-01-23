<template>
  <div className="flex border-2 rounded w-full lg:inline-flex lg:w-80">
    <input
        type="text"
        className="px-4 py-2 w-full"
        :placeholder="placeholder"
        v-model="internalQuery"
        @input="$emit('update:query', internalQuery)"
    >
  </div>
</template>

<script setup>
import {ref, watch, computed} from 'vue';

const props = defineProps({
  query: {
    type: String,
    required: true
  },
  placeholder: {
    type: String,
    default: 'Search for name or email...'
  }
});

const internalQuery = ref(props.query);

const emitQueryUpdate = computed({
  get: () => internalQuery.value,
  set: (val) => {
    internalQuery.value = val;
    emit('update:query', val);
  }
});

watch(() => props.query, (newVal) => {
  internalQuery.value = newVal;
});
</script>