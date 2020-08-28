<template>
   
    <div>
        <h3> MY ACCOUNT </h3>
        <p v-if="$fetchState.pending"> Fetching info... </p>
        <p v-else-if="$fetchState.error"> An error occurred :( </p>
        <div v-else> 
            <table class="my_acc_table" cellspacing="0"> 

                <tr> 
                    <td style="width: 15%; border-right: 1px solid #e5e5e5; vertical-align: text-top; border-bottom: 1px solid #e5e5e5; text-align: center">
                        <p class="row_name"> General Information </p> 
                    </td>

                    <td style="text-align: center; overflow: hidden; border-bottom: 1px solid #e5e5e5"> 
                        <img src="https://cdn.iconscout.com/icon/free/png-512/avatar-370-456322.png" class="profile_avatar" @click="$refs.file.click()" />
                        <p class="avatar_text"> Click to edit </p>
                        <p style="margin-top: 70px;"> Your Avatar </p>
                        <input type="file" ref="file" style="display: none"> 
                    </td>

                    <td style="padding-left: 10px; border-bottom: 1px solid #e5e5e5;"> 
                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> USERNAME: </span> <span class="user_info2"> {{ client.LOGIN }} </span> </p>
                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> FIRST NAME: </span> <span class="user_info2"> {{ client.FIRST_NAME }} </span> </p>
                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> SECOND NAME: </span> <span class="user_info2"> {{ client.SECOND_NAME }} </span> </p>

                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> E-MAIL: </span> <span class="user_info2"> {{ client.EMAIL }} </span > <span @click="openModal('email')"><i class="fas fa-pencil-alt"></i></span> </p> 
                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> PHONE NUMBER: </span> <span class="user_info2"> {{ client.PHONE_NUM }} </span> <span @click="openModal('phone')"><i class="fas fa-pencil-alt"></i></span> </p>
                        <p style="border-bottom: 1px dotted gray"> <span class="user_info"> BIRTH DATE: </span> <span class="user_info2"> {{ client.BIRTH_DATE }} </span> </p>


                    </td>

                </tr>

                <tr> 
                    <td style="width: 15%; border-right: 1px solid #e5e5e5; vertical-align: text-top; border-bottom: 1px solid #e5e5e5; text-align: center">
                        <p class="row_name"> Payment method and credentials </p> 
                    </td>


                    <td colspan='2' style="padding-left: 30px; border-bottom: 1px solid #e5e5e5;"> 
                        blolboblblbo
                    </td>
                </tr>


            </table>


            <!-- The Modal -->
            <div id="myModal" class="modal" v-bind:class="{opened_modal: isOpenedModal}">

                <!-- Modal content -->
                <div class="modal-content">
                    <div class="modal-header">
                        <span @click="openModal" class="close">&times;</span>
                        <h2 v-if="modal_state == 'email'"> Email Change </h2>
                        <h2 v-if="modal_state == 'phone'"> Phone Number Change </h2>
                    </div>
                    <div class="modal-body">
                        <p v-if="modal_state == 'email'"> Please, enter a new email: </p>
                        <p v-if="modal_state == 'phone'"> Please, enter a new phone number: </p>
                        <input v-model="new_info" type="text" style="margin-bottom: 1em;">
                        <button :disabled="new_info == ''" type="button" @click="updateInfo" class="submit_button"> Submit </button>
                    </div>
                </div>

            </div>


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
            client: {},
            isOpenedModal: false,
            new_info: '',
            modal_state: ''
        }
    },

    async fetch() {
        const myurl = 'http://192.168.99.100:1338/api/client/' + this.client_id;

        this.client = await fetch(myurl).then(res => res.json());


    },
    
    methods: {
        openModal: function(state) {
            
            if(state == 'email'){
                this.modal_state = 'email';
            } else if (state == 'phone') {
                this.modal_state = 'phone';
            }
            
            
            this.isOpenedModal = !this.isOpenedModal;

            console.log(this.isOpenedModal);


        },

        updateInfo: async function() {

            const myurl = 'http://192.168.99.100:1338/api/client/' + this.client_id;
            let response;
            try {

                if(this.modal_state == 'email'){
                     response = await this.$axios.put(myurl, {
                        first_name: this.client.FIRST_NAME,
                        second_name: this.client.SECOND_NAME,
                        login: this.client.LOGIN,
                        pass: this.client.PASS,
                        email: this.new_info,
                        phone_num: this.client.PHONE_NUM,
                        birth_date: this.client.BIRTH_DATE.split('T')[0].replaceAll('-', '/')
                    });

                } else if (this.modal_state == 'phone') {
                     response = await this.$axios.put(myurl, {
                        first_name: this.client.FIRST_NAME,
                        second_name: this.client.SECOND_NAME,
                        login: this.client.LOGIN,
                        pass: this.client.PASS,
                        email: this.client.EMAIL,
                        phone_num: this.new_info,
                        birth_date: this.client.BIRTH_DATE.split('T')[0].replaceAll('-', '/')
                    });

                }


                if(response.status == 200){
                    this.isOpenedModal = !this.isOpenedModal;
                    this.new_info = '';
                    this.$fetch();
                } else {
                    this.new_info = '';
                    this.isOpenedModal = !this.isOpenedModal;
                    alert('something went wrong...');
                }


            } catch(err) {
                alert(err);
            }
            


        }


    }

}
</script>
<style scoped>

.profile_avatar {
    height: 150px; 
    width: 150px; 
    opacity: 1;
    margin-top: -15px;
    transition: transform 0.2s, opacity 0.2s;
    -webkit-transition: transform 0.2s, opacity 0.2s;
}

.profile_avatar:hover {
    cursor: pointer;
    opacity: 0.4;
    transform: scale(1.10);
    -webkit-transition: transform 0.6s, opacity 0.6s;
    transition: transform 0.6s, opacity 0.6s;
}

.profile_avatar:active {
    transform: scale(0.9);
    -webkit-transition: transform 0.2s;
    transition: transform 0.2s;
}

.avatar_text {
    z-index: 99;
    margin-top: -85px;
    opacity: 0;
    font-weight: 700;
    font-size: 1em;
    -webkit-transition: opacity 0.4s;
    transition: opacity 0.4s;
}

.profile_avatar:hover ~ .avatar_text {
    opacity: 1;
    -webkit-transition: opacity 1.5s;
    transition: opacity 1.5s;
}

.user_info {
    background-color: #39b982;
    color: white;
    padding: 5px 5px 5px 5px;
    margin-right: 200px;
}

.user_info2 {
    float: right;
    
}

.fa-pencil-alt {
    float: right;
    margin-right: 10px;
    cursor: pointer;
}

.fa-pencil-alt:hover {
    color: gray;
}

.row_name {
    font-size: 1.5em;
}

.submit_button {
    background-color: #39b982; 
    border: none;
    color: white;
    padding: 10px 25px;
    text-align: center;
    text-decoration: none;
    font-size: 15px;
    cursor: pointer;
    
    margin-bottom: 2em;
}

.submit_button:hover {
    background-color: #3fcf91;
}

.submit_button:disabled {
    background-color: #dddddd;
    cursor: initial;
}



/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

.opened_modal {
    display: block;
}

/* Modal Content */
.modal-content {
  position: relative;
  background-color: #fefefe;
  margin: auto;
  padding: 0;
  border: 1px solid #888;
  width: 80%;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
  -webkit-animation-name: animatetop;
  -webkit-animation-duration: 0.4s;
  animation-name: animatetop;
  animation-duration: 0.4s
}

/* Add Animation */
@-webkit-keyframes animatetop {
  from {top:-300px; opacity:0} 
  to {top:0; opacity:1}
}

@keyframes animatetop {
  from {top:-300px; opacity:0}
  to {top:0; opacity:1}
}

/* The Close Button */
.close {
  color: white;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}

.modal-header {
  padding: 2px 16px;
  background-color: #39b982;
  color: white;
}

.modal-body {padding: 2px 16px;}




</style>