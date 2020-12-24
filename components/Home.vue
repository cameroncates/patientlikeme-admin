<template>
    <div class="px-4 py-5">
        <div class="row mx-0 my-4">
            <div class="col l6 m6 s6 p-2 animate__animated animate__zoomIn" v-for="(ana, i) in analytics" :key="i">
                <div class="shadow px-2 py-5 bg-white border-radius-5">
                    <h5 class="text-center">{{ ana.name }}</h5>
                    <a @click="$emit('switchTabTo', ana.tab)" href="#" class="text-success btn btn-block btn-flat transparent text-center font-weight-bold"> {{ ana.value }} </a>
                </div>
            </div>
        </div>        


    </div>
</template>

<script>
export default {

    components: {
    },

    data() {
        return {
            analytics: [
                { name: "Manage Users", value: "Manage", tab: "USERS" },
                { name: "Treatments", value: "Create Or Remove Treatements", tab: "TREATMENTS" },
                { name: "Symptoms", value: "Create Or Remove Symptoms", tab: "SYMPTOMS" },
                { name: "Conditions", value: "Create Or Remove Conditions", tab: "CONDITIONS" },
            ]            
        }
    },
    methods: {
    },

    mounted() {


        
    }
}
</script>

<style>

</style>