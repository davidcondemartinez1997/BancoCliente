<template>
  <div id="maestro">
    <ul v-if="direcciones && direcciones.length">
      <li v-for="domicilio of direcciones" v-on:click="detail" v-bind:id="domicilio.Id">
        {{domicilio.Nombre}} {{domicilio.Numero}}
      </li>
    </ul>
    <input type="button" id="submit" value="Nuevo" v-on:click="nuevo"/>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import {EventBus} from './EventBus.js';
  import Form from './FormDomicilio.vue'
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        direcciones: undefined
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        this.direcciones.forEach((p, index) => {
          if(p.Id == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                domicilio: p
              },
            });
          }
        });
      },
      nuevo: function (e) {
        new Vue({
          el: '#form',
          render: h => h(Form),
          data: {
            domicilio: {
              Nombre: undefined,
              Numero: undefined,
              Localidad: undefined,
              Id:-1
            }
          },
        });

      },
      init: function(){
        axios.get('http://10.60.23.16:62270/api/Domicilio/')
        .then(response => {
          this.direcciones = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('updateDomicilio', (() => {
        this.init();
      }));


  }
}
</script>

<style>
  *{
    padding: 5px;
  }

  li {
    list-style: none;
    border: 1px solid black;
    width: 200px;
  }
</style>
