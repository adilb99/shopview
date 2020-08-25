<template>
    <div> 
        <nuxt-link to="/login" style="font-size: 1.3em; float: left; margin-left: 1em;"> Back </nuxt-link>
        <LoginForm> 
            <form>
            <p style="font-weight: 700; font-size: 1.5em;"> REGISTER </p>
            
            
            <div class="input_container">
                <label> Email: </label> <br>
                <input v-model="email_in" class="input_style" type='text' placeholder="Email"> <br> <br>
            </div>

            <div class="input_container">
                <label> First Name: </label> <br>
                <input v-model="first_in" class="input_style" type='text' placeholder="First Name"> <br> <br>
                
            </div>

            <div class="input_container">
                <label> Last Name: </label> <br>
                <input v-model="last_in" class="input_style" type='text' placeholder="Last Name"> <br> <br>
            </div>

            <div class="input_container">
                <label> Phone Number: </label> <br>
                <input v-model="phone_in" class="input_style" type="text" placeholder="Phone Number"> <br> <br>
            </div>
            
            <div class="input_container">
                <label> Username: </label> <br>
                <input v-model="login_in" @input="checkUsername" class="input_style" type='text' placeholder="Username"> <br> <br> 
                <p v-if="taken" style="color: red; margin-top: -1em"> This username is taken! </p>
            </div>

            <div class="input_container">
                <label> Password: </label> <br>
                <input v-model="password_in" class="input_style" type="password" placeholder="Password">
                <p style="font-size: 0.8em; color: #b5b5b5;"> Make sure the password contains A-Z, a-z, 0-9 </p>
            </div>

            <p v-if="passcheck == 'failed'" style="color: red;"> Password doesn't match the rules! </p>

            <button :disabled="email_in == '' || first_in == '' || last_in == '' || phone_in == '' || login_in == '' || password_in == ''" @click="sendData" type='button' class="login_button"> REGISTER </button>
            
            </form>

            

        </LoginForm>

    </div>
</template>
<script>
import LoginForm from "../components/LoginForm.vue";
import axios from "@nuxtjs/axios";

export default {
    layout: 'login',

    components: {
        LoginForm
    },

    data() {
        return {
            login_in: '',
            password_in: '',
            phone_in: '',
            first_in: '',
            last_in: '',
            email_in: '',
            birth_date_in: '',
            taken: false,
            passcheck: ''
        }
    },

    methods: {
        sendData: async function() {
            let resp;
            
            try {
                resp = await this.$axios.$post('http://192.168.99.100:1338/api/client', {
                    first_name: this.first_in,
                    second_name: this.last_in,
                    login: this.login_in,
                    pass: this.password_in,
                    email: this.email_in,
                    phone_num: this.phone_in,
                    birth_date: '1999/12/02'
                });

                console.log(resp);

                if(resp){
                    alert('Registration succesful!');
                    
                    this.$router.push('/login');
                }

            } catch(err) {
                console.log(err);

                alert(err);
            }

            return { resp };
        },
        checkUsername: async function() {

            // const sleep = ms => new Promise(res => setTimeout(res, ms));

            // await sleep(1500);

            try {
                const response = await this.$axios.$get('http://192.168.99.100:1338/api/client/login/' + this.login_in);
                this.taken = true;
            } catch(err) {
                const response = 'error';
                this.taken = false;
            }
            
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

.input-container label {
    text-align: left;
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
    margin-top: 0.7em;
}

.login_button:hover {
    background-color: #3fcf91;
}

.login_button:disabled {
    background-color: #dddddd;
    cursor: initial;
}
</style>