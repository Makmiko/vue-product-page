<template>
  <div id="app">
    <Products :products="products"/>
  </div>
</template>

<script>
import Products from './components/Products.vue';
import products from './data/products.json';

export default {
  name: 'App',
  components: {
    Products
  },
  data() {
    return {
      products: products
    }
  }
}
</script>

<style>
  #app {
    font-family: Arial,Helvetica,sans-serif;
    font-size: 16px;
  }
</style>
