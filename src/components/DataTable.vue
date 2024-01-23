<template>
  <table class="min-w-full leading-normal">
    <thead>
    <tr>
      <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
        Name
      </th>
      <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
        Email
      </th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="item in items" :key="item.id">
      <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
        {{ item.name }}
      </td>
      <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
        {{ item.email }}
      </td>
    </tr>
    </tbody>
  </table>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
});
</script>