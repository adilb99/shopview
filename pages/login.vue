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
                <input v-model="password_in" class="input_style" type="password" placeholder="Password">
            </div>

            <p> Don't have an account? <nuxt-link to="/register"> Register </nuxt-link> </p>
            
            <button type='button' @click="$fetch" class="login_button"> LOG IN </button>
            
            </form>

        </LoginForm>

        <p> {{ access }} </p>


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
            access: false, 
            password_in: '',


        }
    },

    async fetch() {
        
        const myurl = 'http://192.168.99.100:1338/api/client/login/' + this.login_in;

        this.user = await fetch(myurl).then(res => res.json());
    
        if(this.user.PASS == this.password_in){
            this.access = true;
            this.$router.push('/');
        } else {
            this.access = false;
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

</style>