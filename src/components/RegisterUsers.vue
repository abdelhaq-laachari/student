<template>
    <div>
        <h1>Student Management</h1> 
    </div>

    
    <div class="container">
        <!-- Add Button -->
        <div class="col-lg-12">
            <!-- Add Student Button -->
            <button class="float-end btn btn-info " @click="addModalsDialog(true)">
                <FIcons :icon="['fas','user'] " />&nbsp; Add Student
            </button>
        <!-- Delete All Student Button -->
            <button class="float-start btn btn-danger " v-if="studentData.length > 0" @click="DeleteAllModalsDialog(true)">
                <FIcons :icon="['fas','user-times'] " />&nbsp; Delete All Student
            </button>
            <div class="clearfix"></div>
        </div>
        <hr class="bg-info">

        <!-- Show Error Message -->
        <div class="alert alert-danger alert-dismissible fade show" role="alert" v-if="errorMsg" >
            {{Error}}
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="close" ></button>
        </div>

        <!-- Show Success Message -->
        <div class="alert alert-success alert-dismissible fade show" role="alert" v-if="successMsg" >
            {{Success}}
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="close" ></button>
        </div>

        <!-- Show Student Table -->
        <table class="table table-border table-striped caption-top ">
            <caption>List Of Registered Student ({{studentData.length}}) </caption>
            <thead>
                <tr class="bg-success text-light text-center">
                    <th><FIcons :icon="['fas','id-badge'] " />&nbsp; ID</th>
                    <th><FIcons :icon="['fas','user'] " />&nbsp; Name</th>
                    <th><FIcons :icon="['fas','envelope'] " />&nbsp; Email</th>
                    <th><FIcons :icon="['fas','phone']" />&nbsp; Phone</th>
                    <th><FIcons :icon="['fas','cog'] " />&nbsp; Action</th>
                </tr>
            </thead>
            <tbody>
                <tr class="text-center">
                    <td>1</td>
                    <td>Eric</td>
                    <td>Eric@email.com</td>
                    <td>0612345678</td>
                    <td>
                        <button class="btn btn-danger" @click="DeleteModalsDialog(true)"><FIcons :icon="['fas','user-times'] " />&nbsp; Delete</button>&nbsp; 
                        <button class="btn btn-warning" @click="updateModalsDialog(true)" ><FIcons :icon="['fas','user-edit'] " />&nbsp; Update</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <!-- Add New Student Form -->
        <div id="overlay" v-if="ShowAddModals">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                    <h5 class="modal-title text-info"> Add New Student </h5>
                    <button type="button" class="btn-close" aria-hidden="true" aria-label="Close" @click="addModalsDialog(false)"></button>
                    </div>
                    <div class="modal-body p-4">
                        <form action="#" method="POST" @click.prevent>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentInfo.name.$error } " >
                                <input type="text" class="form-control" id="studentName" placeholder="Eric Parker" v-model="StudentInfo.name" />
                                <label for="studentName"><FIcons :icon="['fas','user'] " />&nbsp; Student Name</label>
                                <span class="error-feedback" v-if="v$.StudentInfo.name.$error" > {{v$.StudentInfo.name.$errors[0].$message}} </span>
                            </div>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentInfo.email.$error } " >
                                <input type="email" class="form-control" id="studentEmail" placeholder="Eric Parker" v-model="StudentInfo.email" />
                                <label for="studentEmail"><FIcons :icon="['fas','envelope'] " />&nbsp; Student Email</label>
                                <span class="error-feedback" v-if="v$.StudentInfo.email.$error" > {{v$.StudentInfo.email.$errors[0].$message}} </span>
                            </div>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentInfo.phone.$error } " >
                                <input type="text" class="form-control" id="studentPhone" placeholder="Eric Parker" v-model="StudentInfo.phone" />
                                <label for="studentPhone"><FIcons :icon="['fas','phone'] " />&nbsp; Student Phone</label>
                                <span class="error-feedback" v-if="v$.StudentInfo.phone.$error" > {{v$.StudentInfo.phone.$errors[0].$message}} </span>
                            </div>
                            <hr class="bg-info">
                            <div class="d-grid gap-2">
                                <button class="btn btn-info" @click="addNewUser()">Add Student</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Update Student Form -->
        <div id="overlay" v-if="ShowUpdateModals">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                    <h5 class="modal-title text-warning"> Update Student Info </h5>
                    <button type="button" class="btn-close" aria-hidden="true" aria-label="Close" @click="updateModalsDialog(false)"></button>
                    </div>
                    <div class="modal-body p-4">
                        <form action="#" method="POST" @click.prevent>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentUpdate.name.$error } " >
                                <input type="text" class="form-control" id="studentName" placeholder="Eric Parker" v-model.trim ="StudentUpdate.name" />
                                <label for="studentName"><FIcons :icon="['fas','user'] " />&nbsp; Student Name</label>
                                <span class="error-feedback" v-if="v$.StudentUpdate.name.$error" > {{v$.StudentUpdate.name.$errors[0].$message}} </span>
                            </div>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentUpdate.email.$error } " >
                                <input type="email" class="form-control" id="studentEmail" placeholder="Eric Parker" v-model.trim ="StudentUpdate.email" />
                                <label for="studentEmail"><FIcons :icon="['fas','envelope'] " />&nbsp; Student Email</label>
                                <span class="error-feedback" v-if="v$.StudentUpdate.email.$error" > {{v$.StudentUpdate.email.$errors[0].$message}} </span>
                            </div>
                            <div class="form-floating mb-3" :class="{'form-group-error' : v$.StudentUpdate.phone.$error } " >
                                <input type="text" class="form-control" id="studentPhone" placeholder="Eric Parker" v-model.trim ="StudentUpdate.phone" />
                                <label for="studentPhone"><FIcons :icon="['fas','phone'] " />&nbsp; Student Phone</label>
                                <span class="error-feedback" v-if="v$.StudentUpdate.phone.$error" > {{v$.StudentUpdate.phone.$errors[0].$message}} </span>
                            </div>
                            <hr class="bg-info">
                            <div class="d-grid gap-2">
                                <button class="btn btn-warning" @click="updateUser()">Update Student Info</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Delete All Student Form -->
        <div id="overlay" v-if="ShowDeleteModals">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                    <h5 class="modal-title text-danger"> Delete Student Info </h5>
                    <button type="button" class="btn-close" aria-hidden="true" aria-label="Close" @click="DeleteModalsDialog(false)"></button>
                    </div>
                    <div class="modal-body p-4">
                        <form action="#" method="POST" @click.prevent>
                            <h6 class="text-danger" >Are You Sure You Want To Delete This Student Info ?</h6>
                            <p>
                                <span><FIcons :icon="['fas','user'] " />&nbsp; Eric</span> <br>
                                <span><FIcons :icon="['fas','envelope'] " />&nbsp; Eric@email.com</span> <br>
                                <span><FIcons :icon="['fas','phone']" />&nbsp; 0612345678</span>
                            </p>
                            <hr class="bg-info">
                            <div class="d-grid gap-2">
                                <button class="btn btn-danger " @click="DeleteUser()">Delete</button>
                                <button class="btn btn-success " @click="DeleteModalsDialog(false)">Cancel</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Delete Student Form -->
        <div id="overlay" v-if="ShowDeleteAllModals">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                    <h5 class="modal-title text-danger"> Delete All Student </h5>
                    <button type="button" class="btn-close" aria-hidden="true" aria-label="Close" @click="DeleteAllModalsDialog(false)"></button>
                    </div>
                    <div class="modal-body p-4">
                        <form action="#" method="POST" @click.prevent>
                            <h6 class="text-danger" >Are You Sure You Want To Delete All Student ?</h6>
                            <p class="text-danger">
                                Deleting all student, will remove from your system.
                            </p>
                            <hr class="bg-info">
                            <div class="d-grid gap-2">
                                <button class="btn btn-danger " @click="DeleteAllUser()">Delete</button>
                                <button class="btn btn-success " @click="DeleteAllModalsDialog(false)">Cancel</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
   
</template>

<script>

import { useVuelidate } from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";

export default{
    name: "RegisterUsers",
    data(){
        return{
            v$: useVuelidate(),
            errorMsg:"",
            successMsg:"",
            studentData:[],
            ShowAddModals: false,
            ShowUpdateModals: false,
            ShowDeleteModals: false,
            ShowDeleteAllModals: false,
            StudentInfo:{
                name:"",
                email:"",
                phone:"",
            },
            StudentUpdate:{},
        };
    },
    validations(){
        return {
            StudentInfo: {
                name : {required, minLength: minLength(5) },
                email : {required, email },
                phone : {required, minLength: minLength(10) },
            },
            StudentUpdate: {
                name : {required, minLength: minLength(5) },
                email : {required, email },
                phone : {required, minLength: minLength(10) },
            },
        }
    },
    methods: {
        addModalsDialog(val){
            this.ShowAddModals = val;
        },
        updateModalsDialog(val){
            this.ShowUpdateModals = val;
        },
        DeleteModalsDialog(val){
            this.ShowDeleteModals = val;
        },
        DeleteAllModalsDialog(val){
            this.ShowDeleteAllModals = val;
        },
        addNewUser(){
            this.v$.StudentInfo.$validate();
            if(!this.v$.StudentInfo.$error)
            {
                console.log("Add New User : Validated Successfully");
            }
            else
            {
                console.log("Add New User : Not Validated");
            }
        },
        updateUser(){
            this.v$.StudentUpdate.$validate();
            if(!this.v$.StudentUpdate.$error)
            {
                console.log("Update User Info : Validated Successfully");
            }
            else
            {
                console.log("Update User Info : Not Validated");
            }
        }
    }
};
</script>







<style scoped>
    h1{
        background-color: cadetblue;
        color: #fff;
        padding: 1rem;
        margin-bottom: 50px;
        text-align: center;
    }

    #overlay{
        position: fixed;
        left: 0;
        right: 0;
        bottom: 0;
        top: 0;
        background-color: rgba(0, 0, 0, 0.5);
    }

    .form-group-error{
        color: red;
        
    }
    .form-group-error input{
        border-color: red;

    }


</style>