<template>
      <div class="col-md-6">

            <form @submit.prevent="onSubmit" @keydown="errors.clear($event.target.name)">

                <div class="form-group">
                    
                    <input type="text" class="form-control" placeholder="Enter Name" v-model="form.name"> <br><br>

                    <span v-if="form.errors.has('name')" v-test="form.errors.get('name')"></span>

                </div>

                <div class="form-group">

                    <input type="email" class="form-control" placeholder="Enter Email" v-model="form.email"><br> <br>

                    <span v-if="form.errors.has('email')" v-test="form.errors.get('email')"></span>

                </div>


                <div class="form-group">

                    <input type="password" class="form-control" placeholder="Enter password" v-model="form.password"><br> <br>

                    <span v-if="form.errors.has('password')" v-test="form.errors.get('password')"></span>

                </div>


                <div class="form-group">

                    <input type="password" class="form-control" placeholder="Confirm password" v-model="form.password_confirmation"><br> <br>

                    <span v-if="form.errors.has('password_confirmation')" v-test="form.errors.get('password_confirmation')"></span>

                </div>

                <button type="submit" :disabled="false">submit</button>
            </form>

        </div>
</template>

<script>

    import axios from 'axios'

    class Errors {

        constructor(){

            this.errors = {};
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


           if (field) delete this.errors[field]; //field was given clear field else reset the whole errors


           this.errors = {};
        }


        has(field){

            return this.errors.hasOwnProperty(field);
        }


        any(){


            return Object.keys(this.errors).length > 0;
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



        reset(){



            for (let field in originalData){

                this[field] = '';
            }


        }

        data(){


           let data = Object.assign({}, this);


           delete data.originalData;
            

           delete data.errors;


           return data


        }



        onSuccess(response){


            console.log(response.data);


        }



        onFail(error){


            error => this.errors.record(error.response.data);

        }


        submit(requestType, url){


            axios[requestType](url, this.data())

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

                password : '',

                password_confirmation : '',


            }),

       }

    },

    methods : {

        onSubmit:() => {

            this.form.submit('POST', 'https://wipap.herokuapp.com/api/register');

        },
        
    },


    mounted(){


    }

}

</script>

<style>

</style>