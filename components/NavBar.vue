<template>
      <div class="nav">
        <nuxt-link to="/" class="brand">iBuy</nuxt-link>
        <nav>
          <nuxt-link :to="{name: 'index', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">Home</nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'cart', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Cart
            <span @updateNav="$fetch" v-if="cart && cart.length != 0" class="cart_badge"> {{ cart.length }} </span>
            </nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'order', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Orders</nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'account', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Account</nuxt-link>&nbsp;|
          <nuxt-link to="/login"> Log out </nuxt-link>
        </nav>
      </div>
    </template>
    <script> 
      export default {
        data() {
          return {
            cart: []
          }
        },

        activated() {
          if(this.$fetchState.timestamp <= Date.now() - 1000){
            this.$fetch();
          }
        },

        async fetch() {

          if(this.$route.params.cart_id) {
            this.cart = await fetch('http://192.168.99.100:1338/api/cart_content?cart_id=' + this.$route.params.cart_id).then(res => res.json());
            console.log(this.cart);
          }

        }

      }


    </script>
    <style scoped>

    .cart_badge {
      border-radius: 50%;
      border: 3px solid white;
      background: red;
      color: white;
      display: block;
      z-index: 99;
      width: 17px;
      height: 17px;
      text-align: center;
      margin-top: -40px;
      font-size: 0.5em;
      margin-left: -10px;
      position: absolute;
    }

    .brand {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 2.5em;
      color: #39b982;
      text-decoration: none;
    }
    .nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 60px;
    }
    .nav .nav-item {
      box-sizing: border-box;
      margin: 0 5px;
      color: rgba(0, 0, 0, 0.5);
      text-decoration: none;
    }
    .nav .nav-item.router-link-exact-active {
      color: #39b982;
      border-bottom: solid 2px #39b982;
    }
    .nav a {
      display: inline-block;
    }

    nav {
      font-size: 1.5em;
    }

    </style>