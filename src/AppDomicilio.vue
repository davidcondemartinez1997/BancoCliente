<template>
  <div id="maestro">
  <table v-if="direcciones && direcciones.length">
      <tr>
        <th>Nombre</th>
        <th>Numero</th>
        <th>Localidad</th>
      </tr>
      <tr v-for="domicilio of direcciones" v-on:click="detail" v-bind:id="domicilio.Id">
        <td>{{domicilio.Nombre}}</td>
        <td> {{domicilio.Numero}}</td>
        <td> {{domicilio.Localidad}}</td>
      </tr>
    </table>

   <button id="submit"  v-on:click="nuevo">Nuevo</button>
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
        if(id == ""){
          id = e.target.parentNode.id;
        }
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
        axios.get('http://10.0.2.2:62270/api/Domicilio/')
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
</style>
