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
            <button @click="create_condition = !create_condition, getConditions()" 
            :class="create_condition ? 'btn-primary shadow px-5' : 'text-primary'"
            class="btn btn-large btn-flat waves-effect waves-light transition transparent font-weight-bold">
                <span class="d-flex align-items-center">
                    <span  class="mdi mdi-plus px-2 mdi-24px transition"></span>
                    <span class="transition">Add Condition</span>
                </span>
            </button>
        </div>
        <create-condition @record_added="create_condition = false, getConditions()" v-if="create_condition" class="animate__animated animate__fadeInUp animate__faster" />
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
                        <tr v-for="(condition, i) in conditions" :key="i">
                            <td class="py-4">{{ i + 1 }}.</td>
                            <td class="py-4">{{ condition.title }}</td>                        
                            <td class="py-4">{{ condition.symptoms }}</td>                        
                            <td class="py-4 text-center">
                                <button @click="c = condition" data-toggle="modal" type="button" data-target="#details-modal"  class="btn btn-primary waves-effect waves-light px-4">View</button>
                            </td>
                            <td class="text-right">
                                <button @click="remove(condition, i)" class="btn mdi mdi-delete btn-flat waves-light waves-effect transparent"></button>
                            </td>  
                        </tr>
                    </tbody>
                </table>           
            </div>

        </div>

    </div>
</template>

<script>
import CreateCondition from './CreateCondition.vue'
export default {
  components: { CreateCondition },

    data() {
        
    return {
            loading: true,
            conditions: [],
            create_condition: false,
            user: {},
            c: {}
        }
    },
    methods: {
        remove(record, i) {
            let ref = "conditions/" + record.key

            M.toast({ html: "Please Wait..." })


            if (! navigator.onLine) {
                M.toast({ html: "You are Offline!" })
                return
            }

            this.firebase_remove_one(ref, (res) => {
                M.toast({ html: "Condition Deleted Successfully" })
                
            },
            (err) => {
                M.toast({ html: err })
                console.log(err, 'my-error')
            })

            this.conditions.splice(i, 1)
        },
        getConditions() {
            this.loading = true
            this.firebase_get("conditions", (conditions) => {
                this.loading = false
                this.conditions = conditions.reverse()

            })

        }
    },
    mounted() {
        
        M.toast({ html: "Fetching Conditions..." })

        this.getConditions()




    }
}
</script>

<style>

</style>