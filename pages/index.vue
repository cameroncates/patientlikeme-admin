<template>
    <div class="container px-5">
        <br>
        <div class="row px-4 my-5 py-5">
            <div class="col l8 push-l2 shadow py-4" style="border-radius:50px;padding:50px; 20px;">
                <h1 class="display-4 py-4 text-center font-dancing-script">
                  <span class="mdi mdi-chart-bubble text-primary"></span>
                  <br>
                  <span class="text-primary font-dancing-script">L</span>OGIN</h1>
                <div class="divider"></div>
                <div class="row px-5 py-2 pt-5">
                    <div class="col s12 bg-light z-depth-1">
                        <div class="input-field border-bottom-0">
                            <input id="username" type="text" class="validate my-0" v-model="username">
                            <label class="pt-1" for="username">Username</label>
                        </div>
                    </div>
                </div>
                <div class="row px-5 py-2">
                    <div class="col s12 bg-light z-depth-1">
                        <div class="input-field border-bottom-0">
                            <input id="password" type="password" class="validate my-0" v-model="password">
                            <label class="pt-1"  for="password">Password</label>
                        </div>
                    </div>
                </div>
                <div class="row px-5 py-2">
                    <div class="col s12 px-0">
                        <div v-if="loading" class="progress" style="height:5px">
                            <div class="indeterminate"></div>
                        </div>                    
                        <div v-else>
                            <button
                                @click="login()" class="btn btn-block btn-large btn-primary waves-effect waves-light">Sign in</button>
                        </div>
                    </div>
                </div>

            </div>
        </div>
        <br>

    </div>
        
</template>

<script>

import * as queryString from 'query-string';
const axios = require('axios')

export default {
    data() {
        return {
            username: null,
            password: null,
            username_: "admin", password_: "admin",
            loading: false,
            user_type: null
            
        }
    },
    methods: {

        login() {
            
            this.loading = true
            if (this.username == this.username_ && this.password == this.password_) {

                M.toast({ html: "Logged In Successfully"})

                this.setCookie("patientlikeme-localstorage", {
                    username: this.username,
                    password: this.password
                })
                
                this.loading = false

                setTimeout(() => {
                    location.href = "/dashboard"
                }, 500);

            } else {
                M.toast({ html: "Incorrect Credentials"})
                this.loading = false

            }


        }
    },

    mounted() {     
        

    }
}
</script>

<style>

</style>