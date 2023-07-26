<template>
    <div>
      <label for="textbox" class="block text-sm font-medium text-gray-700">
        Choices (separate with commas):
      </label>
      <textarea
        v-model="inputValue"
        @input="onInput"
        class="shadow appearance-none border rounded w-full px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      ></textarea>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        inputValue: '',
      };
    },
    methods: {
      onInput() {
        // Split the comma-separated inputs and store them in an array
        const inputsArray = this.inputValue.split(',').map((input) => input.trim());
        // You can now access the individual inputs using 'inputsArray'
        console.log('Multiple Inputs:', inputsArray);
      },
    },
  };
  </script>
  