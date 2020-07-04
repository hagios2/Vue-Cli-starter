<template>
      <div class="col-md-6">

            <form @submit.prevent="onSubmit" @keydown="form.errors.clear($event.target.name)">

                <div class="form-group">
                    
                    <input type="text" name="name" class="form-control" placeholder="Enter Name" v-model="form.name">

                    <small style="color:red;" v-if="form.errors.has('name')" v-text="form.errors.get('name')"></small>

                </div>

                <div class="form-group">

                    <input type="email" name="email" class="form-control" placeholder="Enter Email" v-model="form.email">

                    <small style="color:red;" v-if="form.errors.has('email')" v-text="form.errors.get('email')"></small>

                </div>



                <div class="form-group">

                    <input type="tel" name="phone" class="form-control" placeholder="Enter phone" v-model="form.phone">

                    <small style="color:red;" v-if="form.errors.has('phone')" v-text="form.errors.get('phone')"></small>

                </div>

                
                <div class="form-group">

                    <input type="text" name="location" class="form-control" placeholder="Enter location" v-model="form.location">

                    <small style="color:red;" v-if="form.errors.has('location')" v-text="form.errors.get('location')"></small>

                </div>


                <div class="form-group">

                    <input type="password" name="password" class="form-control" placeholder="Enter password" v-model="form.password">

                    <small style="color:red;" v-if="form.errors.has('password')" v-text="form.errors.get('password')"></small>

                </div>


                <div class="form-group">

                    <input type="password" name="password_confirmation" class="form-control" placeholder="Confirm password" v-model="form.password_confirmation">

                    <small style="color:red;" v-if="form.confirm()" v-text="form.confirm()"></small>

                </div>

                <button type="submit" :disabled="form.errors.any()">submit</button>
            </form>

        </div>
</template>

<script>

    import axios from 'axios'

    class Errors {

        constructor(){

            this.errors = {};
        }

        
        has(field){

            return this.errors.hasOwnProperty(field);
        }


        any(){


            return Object.keys(this.errors).length > 0;
        }


        get(field){

            if (this.errors[field]){

                return this.errors[field][0];
            }
        }


        record(errors){

            this.errors = errors;

        }



        clear(field){


            if (field) {
               
                delete this.errors[field];

                return;
            
            }else{

                this.errors = {};

            }
        }


    }


    class Form{

        constructor(data){


            this.originalData = data;


            for (let field in data){

                this[field] = data[field];
            }


            this.errors = new Errors();

        }



        confirm(){

            
            if(this.originalData.password_confirmation == ''){


               return 'The confirm password field is required';


            }else if(this.originalData.password != this.originalData.password_confirmation){


                return 'Password missmatch';
                
            }


        }



        reset(){



            for (let field in this.originalData){

                this[field] = '';
            }


        }

        data(){


           let data = Object.assign({}, this);


           delete data.originalData;
            
           delete data.errors;

           data['account_type'] = 'personal';

           return data


        }



        onSuccess(response){


            console.log(response.data);


            this.errors.clear();


            this.reset();


        }



        onFail(error){


            this.errors.record(error.response.data.errors);

        }


        submit(requestType, url){


            axios[requestType.toLowerCase()](url, this.data(), {

                headers:{

                    'Accept': 'application/json',

                    'Content-Type': 'application/json',
                }
            })

            .then(this.onSuccess.bind(this))

            .catch(this.onFail.bind(this)); 

        }
    }

export default {


    data: function(){

       return {
           
            form: new Form({

                name : '',

                email : '',

                location : '',

                phone : '',

                password : '',

                password_confirmation : '',

            }),

       }

    },

    methods : {

        onSubmit: function(){

            this.form.submit('POST', 'https://wipap.herokuapp.com/api/register');

        },
        
    },


    mounted(){


    }

}

</script>

<style>

</style>