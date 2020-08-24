<template>
      <div class="nav">
        <nuxt-link to="/" class="brand">iBuy</nuxt-link>
        <nav>
          <nuxt-link to="/">Home</nuxt-link>&nbsp;|
          <nuxt-link to="/cart">My Cart</nuxt-link>&nbsp;|
          <nuxt-link to="/order">My Orders</nuxt-link>&nbsp;|
          <nuxt-link to="/account">My Account</nuxt-link>&nbsp;|
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