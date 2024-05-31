<template>
    <div>
      <input min="0" type="number" @input="handleInput" placeholder="Enter amount">
      <button :disabled="isDisabled" @click="convert">Go</button>
      <button @click="favorite">Favorite</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      changeAmount: {
        type: Function,
        required: true
      },
      convert: {
        type: Function,
        required: true
      },
      favorite: {
        type: Function,
        required: true
      },
      error: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        amount: 0
      };
    },
    computed: {
      isDisabled() {
        return this.error !== '' || this.amount <= 0;
      }
    },
    methods: {
      handleInput(event) {
        this.amount = parseFloat(event.target.value);
        this.changeAmount(this.amount);
      }
    }
  };
  </script>
  
  <style scoped>
  input {
    outline: none;
    border-radius: 3px;
    border: 0;
    padding: 10px 15px;
    background: rgba(173, 127, 127, 0.3);
    color: white;
  }
  
  button {
    margin: 0;
    padding: 10px 15px;
    color: white;
    text-transform: uppercase;
    cursor: pointer;
    background-color: red;
    border: 0;
    border-radius: 3px;
    margin: 5px;
  }
  
  button:disabled {
    background-color: lightgray !important;
    cursor: not-allowed;
  }
  </style>
  