<template>
  <div id="form" v-if="seen">
    <h1>Formulario Domicilio</h1>
    <div>
      <label>Nombre:</label>
      <input type="text" id="nombre" name="nombre" v-bind:value="domicilio.Nombre"/>   
    </div>
    <div>
      <label>Numero:</label>
      <input type="text" id="numero" name="numero" v-bind:value="domicilio.Numero"/>   
    </div>

    <div>
      <label>Localidad:</label>
      <input type="text" id="localidad" name="localidad"v-bind:value="domicilio.Localidad" />   
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
         domicilio: {},
         seen: true
      }
    },
    methods: {
      enviar: function(){
        let data = {
          Nombre: document.getElementById("nombre").value,
          Numero: document.getElementById("numero").value,
          Localidad: document.getElementById("localidad").value,
          Id: this.domicilio.Id
        }
        if(data.Id  = -1){
          axios.post('http://10.60.23.16:62270/api/Domicilio/' ,data)
          .then(response => {
            this.domicilio.Id = response.data.Id;
            EventBus.$emit("updateDomicilio", this.domicilio);
          })
        }
        else{
          axios.put('http://10.60.23.16:62270/api/Domicilio/' + data.Id, data)
          .then(response => {
            EventBus.$emit("updateDomicilio", this.domicilio);
          })
        }

      },
      eliminar: function(){
        this.seen = false;
        axios.delete('http://10.60.23.16:62270/api/Domicilio/' + this.domicilio.Id)
        .then(response => {
          EventBus.$emit("updateDomicilio", this.domicilio);
        })
      }
    },
    created() {
      this.domicilio = this.$parent.domicilio;
    }

  }
</script>

<style>
  *{
    padding: 5px;
  }
</style>
