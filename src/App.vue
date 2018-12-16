<template>
  <div id="app">  
   <cart></cart>
  </div>
</template>

<script>
import cart from "./components/cart/Cart.vue";
export default {
  name: 'app',
  components: {
    cart
  },data(){
    return {
    }
  }
}
</script>
