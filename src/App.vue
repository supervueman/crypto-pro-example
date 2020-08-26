<template>
  <div id="app">
    <h2>Contract form data</h2>

    <div class="app-input">
      <input
        v-model="contract.customerName"
        type="text"
        placeholder="Customer name:"
      >
    </div>

    <div class="app-input">
      <input
        v-model="contract.executorName"
        type="text"
        placeholder="Executor name:"
      >
    </div>
    
    <div class="app-input">
      <input
        v-model="contract.number"
        type="text"
        placeholder="Contract number:"
      >
    </div>

    <div class="app-input">
      <input
        v-model="contract.price"
        type="number"
        placeholder="Price:"
      >
    </div>

    <button @click="sign">Send</button>

    <pre>{{certificates}}</pre>
  </div>
</template>

<script>
import { getUserCertificates, createSignature } from 'crypto-pro';

export default {
  name: 'App',

  data: () => ({
    contract: {
      customerName: '',
      executorName: '',
      number: '',
      price: null
    },
    certificates: []
  }),

  methods: {
    async sign() {
      try {
        this.certificates = await getUserCertificates();

        const hashData = window.btoa(JSON.stringify(this.contract));

        // console.log(hashData)

        const result = await createSignature(this.certificates[0].thumbprint, hashData);

        const decode = atob(result);

        console.log(decode)

        // console.log(result);
      } catch(error) {
        console.log(error)
      }
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 60px;
  text-align: center
}

.app-input {
  margin-bottom: 10px;
}

input {
  height: 40px;
  border-radius: 30px;
  border: 1px solid;
  padding: 10px 20px;
  outline: none;
  width: 250px;
}

button {
  height: 40px;
  padding: 10px 20px;
  background-color: #f71674;
  border: 1px solid #f71674;
  border-radius: 30px;
  color: #ffffff;
  transition: color 0.3s, background-color 0.3s;
  transition-timing-function: ease-in-out;
  cursor: pointer;
  width: 250px;
  margin-top: 15px;
  outline: none;
}

button:hover {
  background-color: #ffffff;
  color: #f71674;
}
</style>
