<template>
  <div id="maestro">

    <table v-if="cuentas && cuentas.length">
      <tr>
        <th>Propietario</th>
        <th>Saldo</th>
        <th>Numero</th>
      </tr>
      <tr v-for="cuenta of cuentas" v-on:click="detail" v-bind:id="cuenta.Id">
        <td>{{cuenta.Propietario}}</td>
        <td> {{cuenta.Saldo}} €</td>
        <td> {{cuenta.Numero}}</td>
      </tr>
    </table>
    <button id="submit"  v-on:click="nuevo">Nuevo</button>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import Form from './FormCuentaBancaria.vue'
  import {EventBus} from './EventBus.js';
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        cuentas: undefined,
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        if(id == ""){
          id = e.target.parentNode.id;
        }
        this.cuentas.forEach((p, index) => {
          if(p.Id == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                cuenta: p
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
            cuenta: {
              Numero: undefined,
              Propietario: undefined,
              Saldo: undefined,
              Id: -1
            }
          },
        });
      },
      init: function(){
        axios.get('http://10.0.2.2:62270/api/CuentaBancaria/')
        .then(response => {
          this.cuentas = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('updateCuenta', () => {
        this.init();
      });

    }

  }
</script>

<style>

</style>
