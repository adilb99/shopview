<template>
    <div> 
        <LoginForm> 
            <form>
            <p style="font-weight: 700; font-size: 1.5em;"> LOGIN </p>
            
            
            <div class="input_container">
                <i class="fas fa-user" style="font-size: 2em; color: #b5b5b5;"></i>
                <input v-model="login_in" class="input_style" type='text' placeholder="Username or Email"> <br> <br>
            </div>
            
            <div class="input_container">
                <i class="fas fa-lock" style="font-size: 2em; color: #b5b5b5"></i>
                <input v-model="password_in" @keyup.enter="$fetch" class="input_style" type="password" placeholder="Password">
            </div>

            <p v-if="access == 'denied'" style="color: red;"> Invalid password or username! </p>

            <p> Don't have an account? <nuxt-link to="/register"> Register </nuxt-link> </p>
            
            <button :disabled="login_in == '' || password_in == ''" type='button' @click="$fetch" class="login_button"> LOG IN </button>
            
            </form>

            

        </LoginForm>



    </div>
</template>
<script>
import LoginForm from "../components/LoginForm.vue";
import '@fortawesome/fontawesome-free/css/all.css';
import '@fortawesome/fontawesome-free/js/all.js';
import axios from "@nuxtjs/axios";

export default {
    layout: 'login',
    components: {
        LoginForm
    },

    data() {
        return {
            login_in: '',
            user: {},
            access: 'not yet', 
            password_in: '',


        }
    },

    async fetch() {
        
        const myurl = 'http://192.168.99.100:1338/api/client/login/' + this.login_in;

        this.user = await fetch(myurl).then(res => res.json());
    
        if(this.user.PASS == this.password_in){
            this.access = 'granted';
            this.$router.push({name: 'index', params: {client_id: this.user.ID, client_name: this.user.FIRST_NAME}});
        } else {
            this.access = 'denied';
        }

    },

    
}
</script>
<style scoped>

input[type="text"], input[type="password"] {
    background-color: #f3f3f3 ;
    width: 85%;
    margin-left: 0.5em;
}

.input-container {
  width: 100%;
  margin-bottom: 20px;
}

.login_button {
    background-color: #39b982; 
    border: none;
    color: white;
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    font-size: 20px;
    cursor: pointer;
}

.login_button:hover {
    background-color: #3fcf91;
}

.login_button:disabled {
    background-color: #dddddd;
    cursor: initial;
}

</style>