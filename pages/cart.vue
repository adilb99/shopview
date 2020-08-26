<template>
<div>
    <h3> MY CART </h3>

    <Sidebar>
      <p id="total_sum"> TOTAL: <span style="color: orange"> {{ total }} KZT </span> </p>

      <form style="margin-top: -1.5em;">
        <p style="font-size: 1.11em; font-weight: 700; margin-bottom: 0.4em; margin-left: 0.7em;"> Fill in your address </p>

        <label class="side_label"> Country </label>
        <input v-model="country" class="side_input" type="text">

        <label class="side_label"> State/Province </label>
        <input v-model="state" class="side_input" type="text">

        <label class="side_label"> City </label>
        <input v-model="city" class="side_input" type="text">

        <label class="side_label"> Street </label>
        <input v-model="street" class="side_input" type="text">

        <label class="side_label"> House </label>
        <input v-model="house" class="side_input" type="text">

        <label class="side_label"> ZIP </label>
        <input v-model="ZIP" class="side_input" type="text">

        <button :disabled="country == '' || state == '' || city == '' || street == '' || house == '' || ZIP == '' || goods.length == 0" @click="submitOrder" type="button" class="buy_button"> To Checkout </button>
      </form>    
    </Sidebar>
    
      <p v-if="$fetchState.pending">Fetching cart contents...</p>
      <p v-else-if="$fetchState.error">An error occured :(</p>
      <div v-else class="cart_list_div">
        <p v-if="goods.length == 0" style="font-size: 2em; text-align: center; margin-top: 200px;"> Nothing here yet... </p>
        <table v-for="good in goods" v-bind:key="good" class="cart_list_table"> 
          <tr class="cart_entry"> 
            <td> <img src='https://www.goldendealsgh.com/wp-content/uploads/2019/04/iPhone-XSmax-Gold.png' alt='smartphone' style='width: 200px; height:200px;' /> </td>
            <td> 
              <span class="delete_button" @click="deleteFromCart(good.CART_CONTENT_ID)"> X </span>
              <p style="font-size: 1.3em;"> 
                <nuxt-link :to="{ name: 'product', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}, query: {id: good.ID}}">{{ good.NAME }}</nuxt-link> 
                &nbsp;&nbsp; <span style="color: #aeaeae"> ({{ good.ID }}) </span> 
              </p>
              <p style="float: right; margin-top: -2.5em; margin-right: 2em;"> Quantity: {{ good.QUANTITY }} </p>
              <p style="font-weight:600; font-size: 1em; margin-top: -15px; color: orange"> {{ good.PRICE }} KZT </p> 
              <p> {{ good.SPEC }} Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat </p>
            </td>
          </tr>
        </table>
      </div>

 </div>
    
</template>
<script>
import Sidebar from '../components/Sidebar.vue';

export default {
  components: {
    Sidebar
  }, 

  data() {
    return {
      cart: [],
      goods: [],
      cart_id: this.$route.params.cart_id,
      client_id: this.$route.params.client_id,
      client_name: this.$route.params.client_name,
      total: 0,
      country: '',
      state: '',
      city: '',
      street: '',
      house: '',
      ZIP: ''
    }
  },

  async fetch() {
    const myurl = 'http://192.168.99.100:1338/api/cart_content?cart_id=' + this.cart_id;

    console.log('cart_id: ' + this.cart_id);
    console.log('client_id: ' + this.client_id)

    if(this.cart_id){
      const response = await fetch(myurl).then(res => res.json());
      this.cart = response;

      for (let i = 0; i < response.length; i++) {
        const element = response[i];
        
        const good = await fetch('http://192.168.99.100:1338/api/product/' + element.PRODUCT_ID).then(res => res.json());

        this.goods.push(Object.assign({}, good, {QUANTITY: element.QUANTITY, CART_CONTENT_ID: element.ID}));

        this.total += good.PRICE * element.QUANTITY;
      }

    } else {
      this.cart = 'Not found :o';
    }
    

  },

  methods: {
    deleteFromCart: async function(content_id) {
      const myurl = 'http://192.168.99.100:1338/api/cart_content/' + content_id;

      try {

        const response = await this.$axios.delete(myurl);

        console.log(response);
        
        for (let i = 0; i < this.cart.length; i++) {
          const element = this.cart[i];
          
          if(element.ID == content_id){
            this.cart.splice(i, 1);
            break;
          }

        }

        for (let i = 0; i < this.goods.length; i++) {
          const element = this.goods[i];

          if(element.CART_CONTENT_ID == content_id){
            this.goods.splice(i, 1);
            this.total -= element.PRICE;
            break;
          }
          
        }

      } catch(err) {
        alert(err);

      }



    },

    submitOrder: async function() {
      
      const url = 'http://192.168.99.100:1338/api/ord';

      try {

        const response = await this.$axios.post(url, {
          cart_id: this.cart_id,
          country: this.country,
          province_state: this.state,
          city: this.city,
          street: this.street,
          house_no: this.house,
          zip: this.ZIP
        });

        if(response.status == 201){
          this.goods = [];
          this.cart = [];
          this.total = 0;

          alert('Transaction succesful!');

          this.$router.push({name: 'order', params: {client_id: this.$route.params.client_id, client_name: this.$route.params.client_name, cart_id: null}});

        } else {
          alert('Error: Something went wrong, try again later...')
        }

        
      } catch(err){
        alert(err);
      }

    }



  }

}
</script>
<style scoped>

.cart_list_div {
  margin-left: 200px;
  margin-top: -2em;
  padding-left: 20px;
}

.side_input {
  margin-bottom: 0.7em;
  margin-left: 0.5em;
  height: 1.2em;
}

.side_label {
  margin-left: 0.75em;
}

.buy_button {
    margin-top: 1.2em;
    margin-left: 2.5em;
    background-color: #39b982; 
    border: none;
    color: white;
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    font-size: 15px;
    cursor: pointer;
}

.buy_button:hover {
  background-color: #3fcf91;
}

.buy_button:disabled {
    background-color: #dddddd;
    cursor: initial;
}

.cart_list_table {
  border-bottom: 1px solid #e5e5e5;
}

#total_sum {
  font-size: 1.11em; 
  font-weight: 700; 
  margin-bottom: 2em; 
  margin-left: 0.6em; 
  margin-top: -0.5em;
  padding-left: 0.5em;
  border-bottom: 1px solid #a8a8a8;
}

.delete_button {
  cursor: pointer;
  color: red;
  float: right;
  margin-top: -0.5em;
  margin-right: 2em;
}

.delete_button:hover {
  color: rgb(255, 124, 124);
}

</style>