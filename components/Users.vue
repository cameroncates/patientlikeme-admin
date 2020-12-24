<template>
    <div class="px-4">
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
                                <h4 class="col s12 border-bottom pb-4 font-weight-bold">User Details</h4>
                            </div>
                            <div class="row px-0" v-if="!u.info && !u.conditions"> <p class="w-100 text-center p-5 m-5">No Details Found</p></div>
                            <div class="row px-0" v-if="u.info">
                                <div class="col s12 m12 l8 px-0 px-md-2">
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">Birthday:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.birthday }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">City:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.city }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">Country:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.country }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">Gender:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.gender }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">Sex:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.sex }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">State:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.state }}</span>
                                    </h6>
                                    <h6 class="font-weight-bold d-flex align-items-center justify-content-between">
                                        <span class="text-super-light">Story:</span>
                                        <span class="text-black font-weight-bold">{{ u.info.story }}</span>
                                    </h6>
                                </div>                       
                            </div>
                            <div class="row px-0" v-if="u.conditions">
                                <div class="col s12 m12 l8 px-0 px-md-2">
                                    <ul>
                                        <li v-for="(condition, i) in u.conditions" :key="i">{{ condition}}</li>
                                    </ul>
                                </div>                       
                            </div>
                        </div>

                </div>
            </div>
        </div>         
        <div v-if="loading" style="min-height:100vh" class="d-flex align-items-center flex-wrap">
            <div class="w-100">
                <div  class="progress" style="height:5px">
                    <div class="indeterminate"></div>
                </div>            
                <p class="w-100 small text-center">Please Wait...</p>
            </div>
        </div>

        <div v-else class="mx-0 my-4 shadow px-3 py-3 bg-white" style="overflow-x: auto;height:100vh">
            <table id="data-table" class="table table-responsive-md w-100 striped" style="overflow: auto;">
                <thead>
                    <tr>
                        <th class="font-weight-bold border-0 py-4">#</th>
                        <th class="font-weight-bold border-0 py-4">Name</th>
                        <th class="font-weight-bold border-0 py-4">Email</th>
                        <th class="font-weight-bold border-0 py-4">Type</th>
                        <th class="font-weight-bold border-0 py-4 text-center">Details</th>
                        <th class="font-weight-bold border-0 py-4 text-center">Remove</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, i) in users" :key="i" class="animate__animated animate__zoomIn">
                        <td class="py-4">{{ i + 1 }}.</td>
                        <td class="py-4">
                            <p class="mb-0 font-weight-bold">
                                <span class="d-flex align-items-center">
                                    <span>{{ user.username }}</span>
                                    <img v-if="user.profile" :src="user.profile" alt="" width="60px" class="px-3">
                                </span>
                            </p>
                        </td>                        
                        <td class="py-4">
                            <p class="mb-0 text-sm text-super-light">{{ user.email }}</p>
                        </td>        
                        <td class="py-4">
                            <p class="mb-0 font-weight-bold text-capitalize" :class="user.type == 'patient' ? 'text-success' : 'text-info'">{{ user.type }}</p>
                        </td>        
                        <td class="py-4 text-center">
                            <button @click="u = user" data-toggle="modal" type="button" data-target="#details-modal"  class="btn btn-primary waves-effect waves-light px-4">View</button>
                        </td>
                        <td class="py-4 text-center">
                            <button @click="remove(user, i)" class="btn mdi mdi-delete transparent btn-flat"></button>
                        </td>
                    </tr>
                </tbody>
            </table>           
        </div>

    </div>
</template>

<script>
export default {

    data() {
        return {
            loading: true,
            users: [],
            u: {},
        }
    },
    methods: {

        remove(user, i) {
            let ref = "users/" + user.key   

            M.toast({ html: "Please Wait..." })


            if (! navigator.onLine) {
                M.toast({ html: "You are Offline!" })
                return
            }

            this.firebase_remove_one(ref, (res) => {
                M.toast({ html: "User Deleted Successfully" })
                
            },
            (err) => {
                M.toast({ html: err })
                console.log(err, 'my-error')
            })

            this.users.splice(i, 1)            
        }
    },
    mounted() {
        
        M.toast({ html: "Fetching Users..." })

        this.firebase_get("/users", (users) => {

            this.loading = false

            for (let i=0; i<users.length; i++) {

                setTimeout(() => {
                    this.users.push(users[i])            

                }, i*100);
            }
            
        })

    }
}
</script>

<style>

</style>