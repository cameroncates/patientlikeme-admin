<template>
    <div class="px-4 position-relative">
    <!-- The Modal -->
        <div class="modal fade bd-example-modal-lg" tabindex="-1" role="dialog"  id="details-modal" aria-hidden="true">
            <div class="modal-dialog  modal-lg">
                <div class="modal-content">
                    

                        <!-- Modal Header -->
                        <div class="modal-header border-0">
                            <button type="button" class="close text-primary mdi mdi-close-circle" data-dismiss="modal"></button>
                        </div>

                        <!-- Modal body -->
                        <div class="modal-body border-none px-0 px-md-4">
                            <div class="row">
                                <h4 class="col s12 border-bottom pb-4 font-weight-bold">Details</h4>
                            </div>
                            <div v-if="c.body" v-html="c.body"></div>
                            <p v-else class="p-5 text-center w-100"> No Details Found!</p>
                        </div>

                </div>
            </div>
        </div> 

        <div class="w-100 text-right py-5 px-0">
            <button @click="create_symptom = !create_symptom, getSymptoms()" 
            :class="create_symptom ? 'btn-primary shadow px-5' : 'text-primary'"
            class="btn btn-large btn-flat waves-effect waves-light transition transparent font-weight-bold">
                <span class="d-flex align-items-center">
                    <span  class="mdi mdi-plus px-2 mdi-24px transition"></span>
                    <span class="transition">Add Symptom</span>
                </span>
            </button>
        </div>
        <create-symptom @record_added="create_symptom = false, getSymptoms()" v-if="create_symptom" class="animate__animated animate__fadeInUp animate__faster" />
        <div v-else>
            <div v-if="loading" style="min-height:75vh" class="d-flex align-items-center flex-wrap">
                <div class="w-100">
                    <div  class="progress" style="height:5px">
                        <div class="indeterminate"></div>
                    </div>            
                    <p class="w-100 small text-center">Please Wait...</p>
                </div>
            </div>                
            <div v-else class="mx-0 my-4 shadow px-3 py-3 bg-white" style="overflow-x: auto; height: 100vh">
                <table id="record-table" class="table table-responsive-md w-100 striped animate__animated animate__zoomIn animate__faster" style="overflow: auto;">
                    <thead>
                        <tr>
                            <th class="font-weight-bold border-0 py-4">#</th>
                            <th class="font-weight-bold border-0 py-4">Name</th>
                            <th class="font-weight-bold border-0 py-4">Symptoms</th>
                            <th class="font-weight-bold border-0 py-4 text-center">Details</th>
                            <th class="font-weight-bold border-0 py-4 text-center">Remove</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(symptom, i) in symptoms" :key="i">
                            <td class="py-4">{{ i + 1 }}.</td>
                            <td class="py-4">{{ symptom.title }}</td>                        
                            <td class="py-4">{{ symptom.symptoms }}</td>                        
                            <td class="py-4 text-center">
                                <button @click="c = symptom" data-toggle="modal" type="button" data-target="#details-modal"  class="btn btn-primary waves-effect waves-light px-4">View</button>
                            </td>
                            <td class="text-right">
                                <button @click="remove(symptom, i)" class="btn mdi mdi-delete btn-flat waves-light waves-effect transparent"></button>
                            </td>  
                        </tr>
                    </tbody>
                </table>           
            </div>

        </div>

    </div>
</template>

<script>
import CreateSymptom from './CreateSymptom.vue'
export default {
  components: { CreateSymptom },

    data() {
        
    return {
            loading: true,
            symptoms: [],
            create_symptom: false,
            user: {},
            c: {}
        }
    },
    methods: {
        remove(record, i) {
            let ref = "symptoms/" + record.key

            M.toast({ html: "Please Wait..." })


            if (! navigator.onLine) {
                M.toast({ html: "You are Offline!" })
                return
            }

            this.firebase_remove_one(ref, (res) => {
                M.toast({ html: "Symptom Deleted Successfully" })
                
            },
            (err) => {
                M.toast({ html: err })
                console.log(err, 'my-error')
            })

            this.symptoms.splice(i, 1)
        },
        getSymptoms() {
            this.loading = true
            this.firebase_get("symptoms", (symptoms) => {
                this.loading = false
                this.symptoms = symptoms.reverse()

            })

        }
    },
    mounted() {
        
        M.toast({ html: "Fetching Symptoms..." })

        this.getSymptoms()




    }
}
</script>

<style>

</style>