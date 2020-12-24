<template>
    <div class="container">
        <div class="row">
            <div class="row mx-0 px-4">
                <h2 class="col pt-4 s12 mb-0 pb-0">Create a condition that you think is necessary</h2>
                <p class="col pb-4 pt-2 text-super-light s12">
                    <span class="mdi mdi-information-outline"></span>
                    Once you add your condition, it will be then visible to all the registered users.
                </p>
                <div class="col s12">
                    <div class="bg-light px-3 py-1">
                        <div class="input-field border-bottom-0">
                            <input type="text" class="validate my-0"  placeholder="Condition Title" v-model="title">
                        </div>
                    </div>
                </div>
                <div class="col s12 my-2"></div>
                <div class="col s12">
                    <div class="bg-light px-3 py-1">
                        <div class="input-field border-bottom-0">
                            <input type="text" class="validate my-0"  placeholder="Symptoms" v-model="symptoms">
                        </div>
                    </div>
                </div>
                <div class="col s12 my-2"></div>
                <div class="col s12">
                    <div class="bg-light px-3 py-1">
                        <div class="input-field border-bottom-0">
                            <textarea id="textarea1" class="materialize-textarea" v-model="body"></textarea>
                            <label for="textarea1">Body </label>                            
                        </div>
                    </div>
                </div>
                <div class="col s12 my-2"></div>
                <div class="col s12 text-right">
                    <div v-if="loading" class="progress" style="height:5px">
                        <div class="indeterminate"></div>
                    </div>                    
                    <button v-else @click="create()" class="btn btn-large btn-primary px-5">Create</button>    
                </div>                


            </div>
        </div>        
    </div>
</template>

<script>
export default {

    data() {
        return {
            loading: false,            
            title: null,
            symptoms: null,
            body: null,
        }
    },

    methods: {
        create() {
            let ref = "conditions",
                obj = {
                title: this.title,
                symptoms: this.symptoms,
                body: this.body
            }
            
            this.loading = true
                M.toast({ html: "Please Wait!" })

            this.firebase_push_db({ ref, obj }, (res) => {

                this.loading = false
                M.toast({ html: "Condition Added Successfully!" })                
            },
            
            (err) => {
                M.toast({ html: err })
                console.log(err, 'my-error')
            })

        }
    },

    mounted() {
        
    }

}
</script>

<style>

</style>