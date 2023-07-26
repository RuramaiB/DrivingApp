<template>
    <div class="mt-2">
      <!-- Upload Button -->
      <label
        for="image-upload"
        class="cursor-pointer px-4 py-2 border-2 border-green-900 text-green-900 hover:text-white rounded-md shadow-sm hover:bg-green-900"
      >
      Upload
      </label>
      
      <input
        type="file"
        id="image-upload"
        ref="fileInput"
        class="hidden"
        @change="onFileSelected"
      />
  
      <!-- Preview Image -->
      <div v-if="imageUrl" class="mt-4">
        <img :src="imageUrl" alt="Uploaded Image" class="max-h-64 rounded-md shadow-md" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        imageUrl: null,
      };
    },
    methods: {
      onFileSelected(event) {
        const file = event.target.files[0];
        this.imageUrl = URL.createObjectURL(file);
      },
    },
  };
  </script>
  