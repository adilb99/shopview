<template>
      <div class="nav">
        <nuxt-link to="/" class="brand">iBuy</nuxt-link>
        <nav>
          <nuxt-link :to="{name: 'index', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">Home</nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'cart', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Cart</nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'order', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Orders</nuxt-link>&nbsp;|
          <nuxt-link :to="{name: 'account', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}">My Account</nuxt-link>&nbsp;|
          <nuxt-link to="/login"> Log out </nuxt-link>
        </nav>
      </div>
    </template>
    
    <style scoped>
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