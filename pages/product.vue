<template>
    <div>
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

        <nuxt-link :to="{name: 'index', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}}"> <p style="float: right; font-size: 1em;"> Back </p> </nuxt-link>
        <p v-if="$fetchState.pending">Fetching goods...</p>
        <p v-else-if="$fetchState.error">An error occured :(</p>
        <div v-else>
            <h2> {{ product.NAME }} </h2>
            <div class="product_image">
                <img src='https://www.goldendealsgh.com/wp-content/uploads/2019/04/iPhone-XSmax-Gold.png' alt='smartphone' style='width: 500px; height:500px;' />    
            </div>
            <div class="product_info"> 
                <p style="font-size: 2em;"> {{ product.NAME }} </p>
                
                <div class="star_rating"> 
                    <span class="fa fa-star checked"></span>
                    <span class="fa fa-star checked"></span>
                    <span class="fa fa-star checked"></span>
                    <span class="fa fa-star"></span>
                    <span class="fa fa-star"></span>
                </div>

                <p style="font-weight: 600; margin-top: -1em; color: orange"> {{ product.PRICE }} KZT </p>
                <p> Category: {{ category.NAME }} </p> 

                <button class="buy_button" @click="addToCart" > Add to Cart </button>
                <button class="buy_button" style="margin-right: 1.5em;"> Buy Now </button>

                <p> Produced by: {{ manufacturer.NAME }} </p>
                

                <p style="color= #d0d0d0; font-size: 1em; "> {{ product.DESCR }} Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum </p>
                


                <div class="product_toggler">
                    <ul class="product_tab_container">
                        <li @click="isActive1 = true; isActive2 = false; isActive3 = false;" class="product_tab" v-bind:class="{ active: isActive1 }">
                            Specs
                        </li>    

                        <li @click="isActive1 = false; isActive2 = true; isActive3 = false;" class="product_tab" v-bind:class="{ active: isActive2 }">
                            Review
                        </li>

                        <li @click="isActive1 = false; isActive2 = false; isActive3 = true;" class="product_tab" v-bind:class="{ active: isActive3 }">
                            Additional Query
                        </li>
                    </ul>

                    <div class="product_tab_body">
                        <div v-if="isActive2" >
                            <table v-for="review in reviews" v-bind:key="review" class="review_item"> 
                                <tr> 
                                   <td style="width: 10%; padding-top: 1.2em;"> 
                                       <img src="https://cdn.iconscout.com/icon/free/png-512/avatar-370-456322.png" style="height: 50px; width: 50px;" />
                                       <p style="margin-top: -0.5em; font-size: 0.7em; margin-left: 0.87em;"> user {{ review.CLIENT_ID }} </p>
                                   </td>


                                   <td style="padding-left: 1em;">
                                       <h3> {{ review.TITLE }} </h3>
                                       <p style="margin-top: -0.4em;"> {{ review.TEXT }} </p>
                                   </td>

                                </tr>
                            </table>
                        </div>
                        <div v-else-if="isActive1"> 
                            <p> {{ product.SPEC }} </p>
                        </div>
                        <div v-else-if="isActive3">

                            <form>

                                <label for="name"> Name: </label>
                                <input type="text" name="name"><br><br>

                                <label for="email">Contact email: </label>
                                <input type="text" name="email"><br><br>

                                <label for="name">Query type: </label>
                                <select> 
                                    <option> Additional product info </option>
                                    <option> Blah Blah </option>
                                    <option> Help </option>
                                </select>
                                
                                <br><br>

                                <label for="name">Your Query: </label>
                                <input type="text" name="name"><br><br>
                                
                                <button disabled> Submit </button>

                            </form>

                        </div>
                        <p v-else> An error occurred... </p>
                    </div>
                
                </div>


            </div>

            

        </div>
    </div>
</template>
<script>
import axios from '@nuxtjs/axios';

export default {
  modules: ['@nuxt/http'],

  components: {
    
  },

  data() {
    return {
      product: {},
      category: {},
      manufacturer: {},
      reviews: [],
      isActive1: true,
      isActive2: false,
      isActive3: false,
      author: {},
      client_id: this.$route.params.client_id,
      client_name: this.$route.params.client_name,
      cart_id: this.$route.params.cart_id
    }
  },

  async fetch() {

    const myurl = 'http://192.168.99.100:1338/api/product/' + this.$route.query.id;
    
    this.product = await fetch(myurl).then(res => res.json());

    const categ = 'http://192.168.99.100:1338/api/categ/' + this.product.CATEG_ID;
    const manuf = 'http://192.168.99.100:1338/api/manuf/' + this.product.MANUFACTURER_ID;
    const rev = 'http://192.168.99.100:1338/api/review?product_id=' + this.product.ID; 

    this.category = await fetch(categ).then(res => res.json());
    this.manufacturer = await fetch(manuf).then(res => res.json());
    this.reviews = await fetch(rev).then(res => res.json());

  },

  methods: {
      addToCart: async function() {
          
        // Creating cart

        try {
            if(this.cart_id){
            
                const res_content = await this.$axios.$post('http://192.168.99.100:1338/api/cart_content', {
                    cart_id: this.cart_id, 
                    product_id: this.$route.query.id, 
                    quantity: 1
                });

                console.log(res_content);

            } else {
                const res_cart = await this.$axios.$post('http://192.168.99.100:1338/api/cart', {client_id: this.client_id});
                const res_content = await this.$axios.$post('http://192.168.99.100:1338/api/cart_content', {
                    cart_id: res_cart.id, 
                    product_id: this.$route.query.id, 
                    quantity: 1
                });

                console.log(res_cart);
                console.log(res_content);
            }

            alert('Product now added to cart!');

        } catch (err) {
            alert('Error: ' + err);
        }
        
        


      },

      buyNow: async function() {
        
      },

  }

}

</script>
<style scoped>

.product_image {
    position: relative;
    float: left;
    border: 1px solid #e5e5e5;
}

.star_rating {
    margin-right: 20%;
    margin-top: -10%;
    float: right;
    cursor: pointer;
}

.star_rating span:hover {
    color: orange;
}

.checked {
  color: orange;
}

.product_info {
    position: relative;
    float: left;
    margin-left: 25px;
    width: 50%;
}

.product_tab_container {
    width: 960px;
    max-width: 100%;
    margin-left: auto;
    margin-right: auto;
    padding-left: 0;
    margin-bottom: 0;
    list-style: none;
    border-top: 1px solid #e5e5e5;
}

.product_tab {
    display: block;
    position: relative;
    padding: 9px 60px 11px;
    float: left;
    font-size: 17px;
    white-space: nowrap;
    cursor: pointer;
    color: black;
    text-decoration: none;
    z-index: 1;
}

.product_tab:hover {
    background-color:  #fbfbfb;
}

.active {
    background-color: #f1f1f1;
}

.product_tab_body {
    border: 1px solid #e5e5e5;
    padding: 10px 10px 10px 10px;
    margin-top: 6.5%;
}


.review_item {
    border-bottom: 1px solid #e5e5e5;
    width: 100%;
}

.buy_button {
    margin-top: -1em;
    margin-right: 1em;
    float: right;
    position: relative;
    background-color: #39b982; 
    border: none;
    color: white;
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    display: block;
    font-size: 15px;
    cursor: pointer;
}

.buy_button:hover {
    background-color: #3fcf91;
}


</style>