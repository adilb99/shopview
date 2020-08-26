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

                            <button class="collapsible" v-bind:class="{opened: isOpened}" @click="openCollapsible"> Leave your review: </button>
                            <div class="my_review" v-bind:class="{my_review_opened: isOpened}">
                                <label> Title: </label>
                                <input type="text" class='my_review_input' placeholder="Your title here..." v-model="myreview_title">

                                <label> Text: </label>
                                <textarea class='my_review_textarea' placeholder="Your review here..." v-model="myreview_text" />
                                
                                <label> Your rating: </label>
                                <div class="review_rating">
                                    
                                    <fieldset class="rating">
                                        <input type="radio" v-model="myreview_rating" id="star5" name="rating" value="5" /><label class = "full" for="star5" title="Awesome - 5 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star4half" name="rating" value="4.5" /><label class="half" for="star4half" title="Pretty good - 4.5 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star4" name="rating" value="4" /><label class = "full" for="star4" title="Pretty good - 4 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star3half" name="rating" value="3.5" /><label class="half" for="star3half" title="Meh - 3.5 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star3" name="rating" value="3" /><label class = "full" for="star3" title="Meh - 3 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star2half" name="rating" value="2.5" /><label class="half" for="star2half" title="Kinda bad - 2.5 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star2" name="rating" value="2" /><label class = "full" for="star2" title="Kinda bad - 2 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star1half" name="rating" value="1.5" /><label class="half" for="star1half" title="Meh - 1.5 stars"></label>
                                        <input type="radio" v-model="myreview_rating" id="star1" name="rating" value="1" /><label class = "full" for="star1" title="Sucks big time - 1 star"></label>
                                        <input type="radio" v-model="myreview_rating" id="starhalf" name="rating" value="0.5" /><label class="half" for="starhalf" title="Sucks big time - 0.5 stars"></label>
                                    </fieldset>
                                    
                                </div>


                                <button type="button" :disabled="myreview_title == '' || myreview_text == ''" @click="leaveReview" class="review_button"> Submit </button>

                            </div>

                            <table v-for="review in reviews" v-bind:key="review" class="review_item"> 
                                <tr> 
                                   <td style="width: 10%; padding-top: 1.2em;"> 
                                       <img src="https://cdn.iconscout.com/icon/free/png-512/avatar-370-456322.png" style="height: 50px; width: 50px;" />
                                       <p style="margin-top: -0.5em; font-size: 0.7em; margin-left: 0.87em;"> user {{ review.CLIENT_ID }} </p>
                                   </td>


                                   <td style="padding-left: 1em;">
                                       <span v-if="review.CLIENT_ID == client_id" class="delete_review_button"> X </span>
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
      cart_id: this.$route.params.cart_id,
      myreview_title: '',
      myreview_text: '',
      myreview_rating: 0.5,
      isOpened: false
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

    this.reviews.reverse();

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

                alert('Product now added to cart!');

            } else {
                const res_cart = await this.$axios.$post('http://192.168.99.100:1338/api/cart', {client_id: this.client_id});
                const res_content = await this.$axios.$post('http://192.168.99.100:1338/api/cart_content', {
                    cart_id: res_cart.id, 
                    product_id: this.$route.query.id, 
                    quantity: 1
                });
                
                this.cart_id = res_cart.id;

                console.log(this.cart_id);

                alert('Product now added to cart!');
            }

            this.$router.push({name: 'cart', params: {
                    client_id: this.$route.params.client_id,
                    client_name: this.$route.params.client_name,
                    cart_id: this.cart_id
                }, query: {id: this.$route.query.id}});
            

        } catch (err) {
            alert('Error: ' + err);
        }
        
        


      },

      buyNow: async function() {
        
      },

      leaveReview: async function() {
          
          const url = 'http://192.168.99.100:1338/api/review';

          try{
              const response = await this.$axios.post(url, {
                client_id: this.client_id,
                product_id: this.product.ID,
                title: this.myreview_title,
                text: this.myreview_text,
                rating: this.myreview_rating
            });

              if(response.status == 201){
                  this.$fetch();
              } else {
                  alert('Something went wrong...');
              }

          } catch(err){
              alert(err);
          }


      },

      openCollapsible: function() {
          this.isOpened = !this.isOpened;
      }

  }

}

</script>
<style scoped>

.collapsible {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
}

.opened, .collapsible:hover {
  background-color: #ccc;
}

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
    word-break: break-all;
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

.collapsible:after {
  content: '\02795'; /* Unicode character for "plus" sign (+) */
  font-size: 13px;
  color: white;
  float: right;
  margin-left: 5px;
}

.opened:after {
  content: "\2796"; /* Unicode character for "minus" sign (-) */
}

.my_review {
    border: 1px solid #e5e5e5;
    box-shadow: 3px 7px 7px #b9b9b9;;
    margin-top: 0em;
    padding: 10px 10px 10px 10px;
    display: none;
    overflow: hidden;
    background-color: #f1f1f1;
}

.my_review_opened {
    display: block;
}

.my_review_input {
    margin-bottom: 1em;
}

.my_review_textarea {
    margin-bottom: 1em;
    resize: none;
    height: 200px;
}

.review_button {
    background-color: #39b982; 
    border: none;
    color: white;
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    font-size: 15px;
    cursor: pointer;
    float: right;
    margin-right: 20px;
    margin-top: -60px;
}

.review_button:hover {
    background-color: #3fcf91;
}

.review_button:disabled {
    background-color: #dddddd;
    cursor: initial;
}

.delete_review_button {
    cursor: pointer;
    color: red;
    float: right;
    margin-bottom: -1em;
}

.delete_review_button:hover {
    color: rgb(255, 96, 96);
}

/* star rating */

.rating { 
  border: none;
  display: inline-block;
}

.rating > input { display: none; } 
.rating > label:before { 
  margin: 5px;
  font-size: 1.25em;
  font-family: FontAwesome;
  display: inline-block;
  content: "\f005";
  cursor: pointer;
}

.rating > .half:before { 
  content: "\f089";
  position: absolute;
}

.rating > label { 
  color: #ddd; 
    float: right;
}

/***** CSS Magic to Highlight Stars on Hover *****/

.rating > input:checked ~ label, /* show gold star when clicked */
.rating:not(:checked) > label:hover, /* hover current star */
.rating:not(:checked) > label:hover ~ label { color: #FFD700;  } /* hover previous stars in list */

.rating > input:checked + label:hover, /* hover current star when changing rating */
.rating > input:checked ~ label:hover,
.rating > label:hover ~ input:checked ~ label, /* lighten current selection */
.rating > input:checked ~ label:hover ~ label { color: #FFED85;  } 

</style>