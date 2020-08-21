<template>
    <div>
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

        <nuxt-link to='/'> <p style="float: right; font-size: 1em;"> Back </p> </nuxt-link>
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


                <p> Category: {{ category.NAME }} </p> 
                <p> Produced by: {{ manufacturer.NAME }} </p>
                <p style="color= #d0d0d0; font-size: 1em; "> {{ product.DESCR }} </p>


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
                        <div v-if="isActive1">
                            <p> {{  }} </p>
                        </div>
                        <div v-else-if="isActive2"> </div>
                        <div v-else-if="isActive3"> </div>
                        <p v-else> An error occurred... </p>
                    </div>
                
                </div>


            </div>

            

        </div>
    </div>
</template>
<script>
export default {
  modules: ['@nuxt/http'],

  components: {
    
  },

  data() {
    return {
      product: {},
      category: {},
      manufacturer: {},
      review: {},
      isActive1: true,
      isActive2: false,
      isActive3: false
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
    this.review = await fetch(rev).then(res => res.json());

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
    color: #39b982;
    text-decoration: none;
    z-index: 1;
}

.product_tab:hover {
    border: 1px solid #e5e5e5;
}

.active {
    background-color: #f0f0f0;
}


</style>