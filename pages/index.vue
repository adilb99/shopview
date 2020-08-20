<template>
  
  <div class="container">
     
    <Sidebar>
     <ul class="sidebar-panel-nav" style="list-style-type:none;">
       <li> Filter items: </li>
       <li> 
         <select @change="$fetch" v-model="selected">
           <option value="" selected> by Category... </option>
           <option value="phone"> SmartPhone </option>
           <option value="watch"> SmartWatch </option>
           <option value="acc"> Accessories </option>
          </select> 
        </li>

       <li>
         <select>
           <option value="" selected> by Price... </option>
           <option value="expensive"> Expensive first </option>
           <option value="cheap"> Cheap first </option>
          </select> 
         </li>
       
       <li>
         <select>
           <option value="" selected> by Popularity... </option>
           <option> Popular first </option>
           <option> Unpopular first </option>
          </select> 
       </li>

     </ul>
   </Sidebar>


    <div class="main">
      
      
      <h2> Welcome to iBuy! </h2>
      
      <button @click="$fetch" id="refresh_button">Refresh</button>

      <p v-if="$fetchState.pending">Fetching goods...</p>
      <p v-else-if="$fetchState.error">An error occured :(</p>
      <div v-else>
        <h3>Product list: </h3> 
        
        <table v-for="good in goods" v-bind:key="good" id="product_table">
          <tr> 
            <td> {{ good.NAME }} </td>
            <td> {{ good }} </td>
          </tr>
        </table>
      </div>

    </div>
    
    
      
  </div>


</template>
<script>
import Sidebar from '../components/Sidebar.vue';
import axios from '@nuxtjs/axios';

export default {
  modules: ['@nuxt/http'],

  components: {
    Sidebar
  },

  data() {
    return {
      goods: [],
      counter: 0,
      selected: ''
    }
  },

  async fetch() {

    if(this.selected == "phone"){
      this.goods = await fetch('http://192.168.99.100:1338/api/product/categ/1').then(res => res.json());
    } else if(this.selected == "watch") {
      this.goods = await fetch('http://192.168.99.100:1338/api/product/categ/2').then(res => res.json());
    } else if(this.selected == "acc") {
      this.goods = await fetch('http://192.168.99.100:1338/api/product/categ/3').then(res => res.json());
    } else {
      this.goods = await fetch('http://192.168.99.100:1338/api/product').then(res => res.json());
    }
    
  }

}

</script>

<style scoped>

  option[value=""][selected] {
    color: gray;

  }

  .sidebar-panel-nav li {
    padding-bottom: 8px;
  }

  #product_table {
    border: 1px solid black;
  }

  #product_table td {
    border: 1px solid black;
  }

  #refresh_button {
    float: right;
    margin-right: 50px;
    cursor: pointer;
  }

  .main {
    margin-left: 200px;
    padding-left: 15px;
    background-color: #f0f0f0;
  }

</style>