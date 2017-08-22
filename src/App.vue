<template>
  <div id="home">
    <div id="usuarios" v-on:click="openComponent">Usuarios</div>
    <div id="domicilios" v-on:click="openComponent">Domicilios</div>
    <div id="cuentas" v-on:click="openComponent">Cuentas bancarias</div>

    <div id="maestro"></div>
  </div>
</template>

<script>
  import axios from "axios"
  import Vue from 'vue'
  //import MaestroUsuario from "usuarios.vue"
  //import MaestroDomicilio from "domicilios.vue"
  //import MaestroCuentas from "cuentas.vue"
  export default {
    name: 'app',
    data () {
      return {

      }
    },
    methods: {
      openComponent: function(e){
        switch(e.target.id){
          case "usuarios": 
          new Vue({
            el: '#maestro',
            render: h => h(App)
          })

          break;
          case "domicilios": 
          new Vue({
            el: '#maestro',
            render: h => h(App)
          })

          break;
          case "cuentas": 
          new Vue({
            el: '#maestro',
            render: h => h(App)
          })

          break;
        }
      }

    },

  }
</script>

<style>
  *{
    padding: 5px;
  }
</style>