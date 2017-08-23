<template>
  <div id="maestro">
    <table v-if="people && people.length">
      <tr>
        <th>Nombre</th>
        <th>Apellidos</th>
        <th>Edad</th>
      </tr>
      <tr v-for="person of people" v-on:click="detail" v-bind:id="person.Id">
        <td>{{person.Nombre}}</td>
        <td> {{person.Apellidos}}</td>
        <td> {{person.Edad}}</td>
      </tr>
    </table>
   <button id="submit"  v-on:click="nuevo">Nuevo</button>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import Form from './FormUsuario.vue'
  import {EventBus} from './EventBus.js';
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        people: undefined
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        if(id == ""){
          id = e.target.parentNode.id;
        }
        this.people.forEach((p, index) => {
          if(p.Id == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                person: p
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
            person: {
              Nombre: undefined,
              Apellidos: undefined,
              Edad: undefined,
              Id:-1
            }
          },
        });

      },
      init: function(){
        axios.get('http://10.0.2.2:62270/api/Usuarios/')
        .then(response => {
          this.people = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('updateUsuario', (() => {
        this.init();
      }));

    }

  }
</script>

<style>
</style>
