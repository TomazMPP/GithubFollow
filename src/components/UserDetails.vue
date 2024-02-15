<template>
    <div class="divUser">
      <h1>ENTER YOUR GITHUB USERNAME</h1>
      <div class="input-group">
        <input v-model="userName" type="text" class="input" placeholder="TomazMPP" autocomplete="off">
        <button class="button--submit" @click="submitUser">Search</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        userName: '',
      };
    },
    methods: {
      submitUser() {
        this.$emit('submit', this.userName);
      },
    },
  };
  </script>

<style scoped>
h1 {
    font-size: 30px;
    font-weight: 800;
    color: #32296b;
}

.divUser {
    align-self: center;
    text-align: center;
}

.input {
  min-height: 50px;
  max-width: 150px;
  padding: 0 1rem;
  color: #000000;
  font-size: 15px;
  border: 1px solid #5e4dcd;
  border-radius: 6px 0 0 6px;
  background-color: white;
}

.button--submit {
  min-height: 50px;
  padding: .5em 1em;
  border: none;
  border-radius: 0 6px 6px 0;
  background-color: #5e4dcd;
  color: #fff;
  font-size: 15px;
  cursor: pointer;
  transition: background-color .3s ease-in-out;
}

.button--submit:hover {
  background-color: #32296b;
}

.input:focus, .input:focus-visible {
  border-color: #32296b;
  outline: none;
}

</style>