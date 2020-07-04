<template>
  <div id="app">

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
<!-- 
    <message></message> -->


    <register></register>

  </div>
</template>



<script>

  import Message from './components/Message.vue'
   import Register from './components/Register.vue'

  export default {

    name: 'app',

    components: {Message, Register},

    data:function(){

      return {



      }


    }
 

  }

</script>

<style>

</style>
