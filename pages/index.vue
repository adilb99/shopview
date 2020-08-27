<template>
  
  <div class="container">
     
    <Sidebar>
     <ul class="sidebar-panel-nav" style="list-style-type:none;">
       <li> Filter items: </li>
       <li> 
         <select @change="$fetch" v-model="select_categ">
           <option value="" selected> by Category... </option>
           <option value="phone"> SmartPhone </option>
           <option value="watch"> SmartWatch </option>
           <option value="acc"> Accessories </option>
          </select> 
        </li>

        <li>
         <select @change="$fetch" v-model="select_manuf">
           <option value="" selected> by Manufacturer... </option>
           <option value="apple"> Apple </option>
           <option value="samsung"> Samsung </option>
          </select> 
       </li>

      <li> Sort items: </li>

       <li>
         <select @change="$fetch" v-model="price_sort">
           <option value="" selected> by Price... </option>
           <option value="desc"> Expensive first </option>
           <option value="asc"> Cheap first </option>
          </select> 
         </li>
       
        <li>
          <select @change="$fetch" v-model="rating_sort">
            <option value="" selected> by Rating... </option>
            <option value="desc"> High rating first </option>
            <option value="asc"> Low rating first </option>
          </select>
        </li>

       <li> 
         <p @click="clearSelects" class="clear_button"> Clear </p>
       </li>

     </ul>
   </Sidebar>


    <div class="main">
      
      
      <h2> Welcome to iBuy, {{ client_name }}! </h2>
      
      <button @click="$fetch" id="refresh_button">Refresh</button>

      <p v-if="$fetchState.pending">Fetching goods...</p>
      <p v-else-if="$fetchState.error">An error occured :(</p>
      <div v-else>
        <h3>Product list: </h3> 

        <div v-for="good in goods" v-bind:key="good" id="product_table">
          <div class="product_card">  
            
            <div class="product_image_wrapper"> 
              <img src='https://www.goldendealsgh.com/wp-content/uploads/2019/04/iPhone-XSmax-Gold.png' alt='smartphone' style='width: 180px; height:180px' /> 
            </div>
            
            <div class="product_info_wrapper"> 
              <p style="font-size: 1.4em;">  <nuxt-link :to="{ name: 'product', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}, query: {id: good.ID}}"> {{ good.NAME }} </nuxt-link> &nbsp;&nbsp; <span style="color: #9c9c9c"> ({{ good.ID }}) </span> </p>
              
              <p style="font-weight:600; font-size: 1em; margin-top: -15px;"> {{good.PRICE}} KZT 

                <div class="star_rating"> 
                    <span class="fa fa-star" v-bind:class="{checked: getScore1(good.RATING)}"></span>
                    <span class="fa fa-star" v-bind:class="{checked: getScore2(good.RATING)}"></span>
                    <span class="fa fa-star" v-bind:class="{checked: getScore3(good.RATING)}"></span>
                    <span class="fa fa-star" v-bind:class="{checked: getScore4(good.RATING)}"></span>
                    <span class="fa fa-star" v-bind:class="{checked: getScore5(good.RATING)}"></span>
                </div>

              </p>
              
              <p style="font-size: 0.8em;"> {{ good.SPEC }} </p>

            </div>

          </div>

        </div>

      </div>

    </div>
    
    
      
  </div>


</template>
<script>
import Sidebar from '../components/Sidebar.vue';
import axios from '@nuxtjs/axios';
import '@fortawesome/fontawesome-free/css/all.css';
import '@fortawesome/fontawesome-free/js/all.js';

export default {
  modules: ['@nuxt/http'],

  components: {
    Sidebar
  },

  data() {
    return {
      goods: [],
      counter: 0,
      select_categ: '',
      price_sort: '',
      select_manuf: '',
      rating_sort: '',
      client_id: this.$route.params.client_id,
      client_name: this.$route.params.client_name,
    }
  },

  async fetch() {

    let myurl = 'http://192.168.99.100:1338/api/product';

    

    if(this.select_categ == "phone"){
      
      myurl += '?categ_id=1';

    } else if(this.select_categ == "watch") {
      
      myurl += '?categ_id=2';

    } else if(this.select_categ == "acc") {
      
      myurl += '?categ_id=3';

    }

    if(myurl.includes('?')) {
      myurl += '&';
    } else {
      myurl += '?';
    }

    if(this.price_sort == 'asc'){
      
      myurl += 'price_sort=asc';
    
    } else if(this.price_sort == 'desc') {
      
      myurl += 'price_sort=desc';
    
    }

    if(myurl.includes('?')) {
      myurl += '&';
    } else {
      myurl += '?';
    }

    if(this.select_manuf == 'apple') {
      myurl += 'manuf_id=1';
    } else if(this.select_manuf == 'samsung') {
      myurl += 'manuf_id=2';
    }

    
    this.goods = await fetch(myurl).then(res => res.json());

  },

  methods: {
    clearSelects: function(){
      this.select_categ = "";
      this.select_manuf = "";
      this.price_sort = "";
      this.$fetch();
    },

    getScore1: function(rating){
      if(rating > 0.5) {
        return true;
      } else {
        return false;
      }
    },

    getScore2: function(rating){
      if(rating>1.5){
        return true;
      } else {
        return false;
      }
    },


    getScore3: function(rating){
      if(rating>2.5){
        return true;
      } else {
        return false;
      }
    },


    getScore4: function(rating){
      if(rating>3.5){
        return true;
      } else {
        return false;
      }
    },

    getScore5: function(rating){
      if(rating>4.5){
        return true;
      } else {
        return false;
      }
    },

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
    margin-left: 50px;
  }

  .product_card {
    position: relative;
    float: left;
    width: 250px;
    height: 350px;
    border: 1px solid #e5e5e5;
    box-sizing: border-box;
    padding: 10px 10px 10px;
  }

  .product_info_wrapper {
    text-align: justify;
  }

  .product_image_wrapper {
    text-align: center;
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
    box-shadow: 3px 7px 7px #b9b9b9;
  }

  .clear_button {
    cursor: pointer;
    color: gray;
    display: inline;
  }

  .clear_button:hover {
    color: rgb(180, 180, 180);
  }

  .star_rating {
    float: right;
    margin-top: -40px;
    margin-right: 10px;
  }

  .checked {
    color: orange;
  }

</style>