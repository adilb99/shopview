<template>
    <div class='container'>
        
        <Sidebar>
            <p style="margin-left: 0.5em; color: black; font-weight: 700; font-size: 1.5em; margin-top: -0.8em;"> Filter: </p>

            <label style="margin-left: 0.5em; margin-bottom: 0.5em; color: black"> By Order Status </label>
            <select class="side_select" @change="filterStatus" v-model="status_filter"> 
                <option value="" selected> Order status </option>
                <option value='1'> Waiting for confirmation </option>
                <option value='2'> Cancelled </option>
                <option value='3'> Completed </option>
                <option value='4'> Preparing for shipment </option>
                <option value='5'> Shipped </option>
                <option value='6'> Out for delivery </option>/
            </select>

            <br>

            <label style="margin-left: 0.5em; margin-bottom: 0.5em; color: black"> By Date </label>  <br> 
            <label class="side_label"> From: </label>
            <input type="date" class="side_select" v-model="date_filter_pre">

            <label class="side_label"> To: </label>
            <input type="date" class="side_select" v-model="date_filter_post"> 

            <p class="clear_button" @click="clearFilters"> Clear </p>

        </Sidebar>
        
        <div class='main'>
            <h3> MY ORDERS </h3>
            <p v-if="$fetchState.pending"> Fetching info... </p>
            <p v-else-if="$fetchState.error"> An error occurred :( </p>
            <div v-else> 
                <table v-for="order in orders" v-bind:key="order" style="border-bottom: 1px solid #e5e5e5"> 
                    <tr class="order_entry">
                        <td style="border-right: 1px solid #e5e5e5; width: 100px; vertical-align: text-top">  
                            <p> ORDER {{ order.ID }} </p>
                        </td>
                        <td style="padding-left: 10px;">
                            <p> <span style="font-weight: 700"> PRODUCTS: </span> </p>
                            
                            <ul v-for="product in order.PRODUCTS" v-bind:key="product" style="list-style: none;"> 
                                <li style="padding: 10px 10px 10px 10px;"> 
                                    <img src='https://www.goldendealsgh.com/wp-content/uploads/2019/04/iPhone-XSmax-Gold.png' alt='smartphone' style='width: 100px; height:100px; border: 1px solid #e5e5e5; vertical-align: text-top; float: left; position: relative' />
                                    
                                    <span style="font-size: 1.3em; padding-left: 0.5em;"> <nuxt-link :to="{ name: 'product', params: {client_id: $route.params.client_id, client_name: $route.params.client_name, cart_id: $route.params.cart_id}, query: {id: product.ID}}"> {{ product.NAME }} </nuxt-link> </span> 
                                    <br>   

                                    <div style="padding-left: 1em; display: inline"> 
                                        <span class="fa fa-star checked"></span>
                                        <span class="fa fa-star checked"></span>
                                        <span class="fa fa-star checked"></span>
                                        <span class="fa fa-star"></span>
                                        <span class="fa fa-star"></span>
                                    </div>

                                    <br>

                                    <span style="font-size: 0.9em; font-weight: 600; padding-left: 1.2em;"> {{ product.PRICE }} KZT </span>
                                    
                                    
                                </li>

                            </ul>

                            <br> 

                            <p> <span style="font-weight: 700"> TOTAL BILL: </span> <span style="color: orange;"> {{ order.BILL }} KZT </span> </p>

                            <p> <span style="font-weight: 700"> DATE OF ORDER: </span> {{ new Date(Date.parse(order.CREATE_DATE)).toString() }} </p>

                            <p> <span style="font-weight: 700"> DELIVERY ADDRESS: </span> 
                                {{ order.ADDRESS.COUNTRY }}, {{ order.ADDRESS.PROVINCE_STATE }}, 
                                {{ order.ADDRESS.CITY }}, {{ order.ADDRESS.STREET }}, {{ order.ADDRESS.HOUSE_NO }},
                                {{ order.ADDRESS.ZIP }}    
                             </p>
                            
                            
                            
                            <p> <span style="font-weight: 700"> STATUS: </span> 
                                <span v-if="order.ORD_STATUS_ID == 3" style="color: green; font-weight: 500;"> {{ order.STATUS }} </span>
                                <span v-else-if="order.ORD_STATUS_ID == 1" style="color: black; font-weight: 500;"> {{ order.STATUS }} </span>
                                <span v-else-if="order.ORD_STATUS_ID == 2" style="color: red; font-weight: 500;"> {{ order.STATUS }} </span>
                                <span v-else style="color: blue; font-weight: 500;"> {{ order.STATUS }} </span> 
                            </p>
            


                        </td>
                    </tr>
                </table>

            </div>

        </div>
    </div>
</template>
<script>
import Sidebar from "../components/Sidebar.vue";

export default {
    components: {
        Sidebar
    },

    data() {
        return {
            cart_id: this.$route.params.cart_id,
            client_id: this.$route.params.client_id,
            client_name: this.$route.params.client_name,
            orders: [],
            status_filter: "",
            date_filter_pre: "",
            date_filter_post: ""
        }
    },

    async fetch() {

        let additional_url;

        let ord_url = 'http://192.168.99.100:1338/api/ord?client_id=' + this.client_id;

        if(this.status_filter !== "") {
            additional_url = '&ord_status_id=' + this.status_filter;
            ord_url += additional_url;
        }

        const ords = await fetch(ord_url).then(res => res.json());
        
        for (let i = 0; i < ords.length; i++) {
            const element = ords[i];
            
            const cart_cont_url = 'http://192.168.99.100:1338/api/cart_content_history?cart_id=' + element.CART_HISTORY_ID;

            const cart = await fetch(cart_cont_url).then(res => res.json());
            const products = [];

            for (let j = 0; j < cart.length; j++) {
                const element2 = cart[j];

                const product = await fetch('http://192.168.99.100:1338/api/product/' + element2.PRODUCT_ID).then(res => res.json());

                products.push(product);
            }

            const address = await fetch('http://192.168.99.100:1338/api/address/' + element.ADDRESS_ID).then(res => res.json());

            const status = await fetch('http://192.168.99.100:1338/api/order_status/' + element.ORD_STATUS_ID).then(res => res.json());

            this.orders.push(Object.assign({}, element, {PRODUCTS: products, ADDRESS: address, STATUS: status.NAME}));

        }

        this.orders.reverse();

    },

    methods: {
        filterStatus: function() {

           this.orders = [];
           this.$fetch();

        },

        clearFilters: function() {

            this.status_filter = "";
            this.orders = [];
            this.$fetch();

        }
    }
}
</script>
<style scoped>

option[value=""][selected] {
    color: gray;
  }

.main {
    margin-left: 200px;
}

.side_select {
    margin-left: 1em;
    margin-bottom: 1em;
}

.side_label {
    margin-left: 1.5em;
}

.clear_button {
    cursor: pointer;
    color: gray;
    margin-left: 1em;
    display: inline;
  }

  .clear_button:hover {
    color: rgb(180, 180, 180);
  }


  .fa-star {
      cursor: pointer;
  }

  .fa-star:hover {
      color: orange;
  }

  .checked {
      color: orange;
  }

</style>