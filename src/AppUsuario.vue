<template>
  <div id="maestro">
    <ul v-if="people && people.length">
      <li v-for="person of people" v-on:click="detail" v-bind:id="person.Id">
        {{person.Nombre}} {{person.Apellidos}}
      </li>
    </ul>
    <input type="button" id="submit" value="Nuevo" v-on:click="nuevo"/>
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
        axios.get('http://10.60.23.16:62270/api/Usuarios/')
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
  *{
    padding: 5px;
  }

  li {
    list-style: none;
    border: 1px solid black;
    width: 200px;
  }
</style>
