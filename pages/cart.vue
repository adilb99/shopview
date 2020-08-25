<template>
<div>
    <h1> MY CART HERE </h1>
    <p> {{ response }} </p>
 </div>
    
</template>
<script>
export default {

  data() {
    return {
      cart: {},
      cart_id: this.$route.params.cart_id,
      client_id: this.$route.params.client_id,
      client_name: this.$route.params.client_name
    }
  },

  async fetch() {
    const myurl = 'http://192.168.99.100:1338/api/cart/' + this.cart_id;


    


  }

}
</script>