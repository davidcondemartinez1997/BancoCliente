<template>
  <div id="form" v-if="seen">
    <h1>Formulario</h1>
    <div>
      <label>Numero:</label>
      <input type="text" id="numero" name="numero" v-bind:value="cuenta.Numero"/>   
    </div>
    <div>
      <label>Propietario:</label>
      <input type="text" id="propietario" name="propietario" v-bind:value="cuenta.Propietario"/>   
    </div>

    <div>
      <label>Saldo:</label>
      <input type="number" id="saldo" name="saldo"v-bind:value="cuenta.Saldo" />   
    </div>
    

    <input type="button" id="submit" value="Enviar" v-on:click="enviar"/>
    <input type="button" id="submit" value="Eliminar" v-on:click="eliminar"/>
  </div>
</template>

<script>
  import axios from 'axios';
  import {EventBus} from './EventBus.js';
  export default {
    name: 'app',
    data () {
      return {
       cuenta: {},
       seen: true
     }
   },
   methods: {
    enviar: function(){
      let data = {
        Numero: document.getElementById("numero").value,
        Propietario: document.getElementById("propietario").value,
        Saldo: document.getElementById("saldo").value,
        Id: this.cuenta.Id
      }
      if(data.Id == -1){
        axios.post('http://10.0.2.2:62270/api/CuentaBancaria', data)
        .then(response => {
          this.cuenta.Id = response.data.Id;
          EventBus.$emit("updateCuenta", this.cuenta);
        })
      }else{
        axios.put('http://10.0.2.2:62270/api/CuentaBancaria/' + data.Id, data)
        .then(response => {
          EventBus.$emit("updateCuenta", this.cuenta);
        })
      }
      
    },
    eliminar: function(){
      this.seen = false;
      axios.delete('http://10.0.2.2:62270/api/CuentaBancaria/' + this.cuenta.Id)
      .then(response => {
        EventBus.$emit("updateCuenta", this.cuenta);
      })
    }
  },
  created() {
    this.cuenta = this.$parent.cuenta;
    }

  }
</script>

<style>
</style>
