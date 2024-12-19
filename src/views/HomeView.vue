<template>
    <div class="home">
      <h1>Vue Calculator</h1>
      <CalculatorData />
    </div>
  </template>
  
  <script>
  import CalculatorData from '@/components/CalculatorData.vue'
  
  export default {
    name: 'HomeView',
    components: {
        CalculatorData
    }
  }
  </script>