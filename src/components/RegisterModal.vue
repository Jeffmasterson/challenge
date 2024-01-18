
<template>
  <div>
    <div class="flex min-h-full items-end mt-5">
      <button type="button" class="inline-flex w-full justify-center rounded-md bg-blue-500 hover:bg-blue-700 px-3 py-2 text-sm font-semibold text-white shadow-sm sm:ml-3 sm:w-auto" @click="modalOpen = true">Add Record</button>
    </div>
    <div v-if="modalOpen" class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
      <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
      <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
            <div class="bg-white px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
              <div class="px-8 pt-6 mb-4">
                <h2 class="text-4xl font-extrabold">Add New Record</h2>
                <div v-if="submissionSuccess" class="p-4 mb-4 text-sm text-green-800 rounded-lg bg-green-50">
                  Form submitted successfully!
                </div>
              </div>
              <div class="sm:flex sm:items-start">
                <form class="bg-white rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="onSubmit">
                  <label for="name">Name</label>
                  <input class="mb-5 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="name" v-model="localFormData.name" required>
                  <label for="email">Email</label>
                  <input class="mb-5 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="email" v-model="localFormData.email" required>
                  <label for="country">Country</label>
                  <input class="mb-5 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="country" v-model="localFormData.country" required>
                  <label for="region">Region</label>
                  <input class="mb-5 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="region" v-model="localFormData.region" required>
                  <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                    Add Record
                  </button>
                </form>
              </div>
            </div>
            <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
              <button v-if="submissionSuccess" @click="closeModal" type="button" class="ml-0 mt-3 inline-flex w-full justify-center rounded-md px-3 py-2 text-sm font-semibold shadow-sm ring-1 ring-inset ring-gray-300 bg-red-500 text-white hover:bg-red-700 sm:mt-0 sm:ml-3 sm:w-auto">Close</button>
              <button @click="closeModal" type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto">Cancel</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    formData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      modalOpen: false,
      submissionSuccess: false,
      localFormData: { ...this.formData }
    };
  },
  methods: {
    onSubmit() {
      this.$emit('addRecord', this.localFormData);
      this.submissionSuccess = true;
      this.resetForm();
    },
    resetForm() {
      this.localFormData = { name: '', email: '', country: '', region: '' };
    },
    closeModal() {
      this.resetForm();
      this.modalOpen = false;
      this.submissionSuccess = false
    }
  }
};
</script>
