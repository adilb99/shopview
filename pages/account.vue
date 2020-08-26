<template>
   
    <div>
        <h3> MY ACCOUNT </h3>
        <p v-if="$fetchState.pending"> Fetching info... </p>
        <p v-else-if="$fetchState.error"> An error occurred :( </p>
        <div v-else> 
            <p> {{ client }} </p>

        </div>
    </div>

</template>
<script>

export default {

    data() {
        return {
            cart_id: this.$route.params.cart_id,
            client_id: this.$route.params.client_id,
            client_name: this.$route.params.client_name,
            client: {}
        }
    },

    async fetch() {
        const myurl = 'http://192.168.99.100:1338/api/client/' + this.client_id;

        this.client = await fetch(myurl).then(res => res.json());


    }

}
</script>
<style scoped>



</style>