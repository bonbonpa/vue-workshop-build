<template>
  <!-- when using a component in a template Vue.js expects the DOM element to be
  lowercased and dasherized. e.g. ProductList -> product-list -->
 <product-list></product-list>
  <!-- use the <product-list> component here! -->
</template>

<script>
// import product list component
import ProductList from './components/ProductList'

export default {
  components: {
    // With ES6 we can use the Object Literal Shorthand Syntax to save us some
    // keystrokes
    ProductList
    // add product list as a component

  }
}
</script>
