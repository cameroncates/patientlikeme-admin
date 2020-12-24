<template>
    <div>

        <!-- The Modal -->
        <div class="modal fade" id="logout-modal">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    
                    <!-- Modal body -->
                    <div class="modal-body border-none">
                        <h5 class="d-block text-center text-primary font-weight-bold my-5">Are you sure you want to Logout?</h5>
                        <div class="w-100 text-center mb-4">
                            <button @click="cancelLogout()" class="btn px-4 border-radius bg-light shadow text-primary mx-2 font-weight-bold">Cancel</button>
                            <button @click="logoutUser()" class="btn px-4 waves-effect waves-light  border-radius z-depth-2 btn-primary mx-2">Logout</button>
                        </div>                        
                    </div>

                </div>
            </div>
        </div>
        


        <div class="row position-relative py-0 my-0">
            
            <div :class="showSideNav ? 's12 m4 l2' : 'd-none'" class="col bg-primary py-4 px-0 shadow position-relative"  style="min-height:100vh">
                <div 
                    :class="[tab == link.value ? 'white' : '', (i+1) < navLinks.length ? 'border-bottom-white' : '']" 
                    :data-toggle="link.value == 'LOGOUT' ? 'modal' : ''" :data-target="link.value == 'LOGOUT' ? '#logout-modal' : ''"

                    class="py-3 pl-4" v-for="(link, i) in navLinks" :key="i">
                    <button 
                        @click="link.value != 'LOGOUT' ? tab = link.value : null"
                        :class="[tab == link.value ? 'text-primary font-weight-bold' : 'text-white']"
                        class="btn btn-block transparent text-left btn-flat waves-effect waves-light">
                        {{ link.name }}
                    </button>
                </div>
                
            </div>

            <div class="col s12 m8 l10">                
                <dashboard-home @switchTabTo="tab = $event" v-if="tab == 'DASHBOARD'" />
                <users v-else-if="tab == 'USERS'" />
                <condition v-else-if="tab == 'CONDITIONS'" />
                <treatment v-else-if="tab == 'TREATMENTS'" />
                <symptoms v-else-if="tab == 'SYMPTOMS'" />

               <!--  <tollbooth v-else-if="tab.value == 'TOOL_BOOTH'" />
                <user v-else-if="tab.value == 'CREATE_USER'" />
                <manage-complaints v-else-if="tab.value == 'MANAGE_COMPLAINTS'" /> -->
                
            </div>            
        </div>

    </div>
</template>

<script>
import Condition from '../components/Condition.vue'

import Home from '../components/Home'
import Symptoms from '../components/Symptoms.vue'
import Treatment from '../components/Treatment.vue'
import Users from '../components/Users.vue'

export default {
    components: {
        "dashboard-home": Home,
        Users,
        Condition,
        Treatment,
        Symptoms
    },
    data() {
     
        return {
            showSideNav: true,
            modal: null,
            tab: {},
            navLinks: [],
        }
    },
    watch: {
        tab(n, o) {
        }
    },
    methods: {
        cancelLogout() {
            $("div").click()
        },
        logoutUser() {

            this.setCookie("rcs-localstorage", {}, 14)

            setTimeout(() => {
                location.href = "/"
            }, 500);
        }
    },
    mounted() {


        let navLinks = [
            { name: "Dashboard", value: "DASHBOARD", },
            { name: "Users", value: "USERS", },
            { name: "Treatments", value: "TREATMENTS" },
            { name: "Conditions", value: "CONDITIONS"},
            { name: "Symptoms", value: "SYMPTOMS"},

        ]
    

        this.navLinks = navLinks
        this.tab = this.navLinks[0].value

        this.navLinks.push({
            name: "Logout",
            value: "LOGOUT"
        })
        


        // SETTING UP DATA...

    }
}
</script>

<style>
.modal {
    background-color: transparent !important;
    box-shadow: none !important;
}
</style>