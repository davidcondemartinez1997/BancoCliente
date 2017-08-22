<template>
  <div id="form" v-if="seen">
    <h1>Formulario</h1>
    <div>
      <label>Nombre:</label>
      <input type="text" id="nombre" name="nombre" v-bind:value="person.Nombre"/>   
    </div>
    <div>
      <label>Apellidos:</label>
      <input type="text" id="apellidos" name="apellidos" v-bind:value="person.Apellidos"/>   
    </div>

    <div>
      <label>Edad:</label>
      <input type="number" id="edad" name="edad"v-bind:value="person.Edad" />   
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
         person: {},
         seen: true
      }
    },
    methods: {
      enviar: function(){
        let data = {
          Nombre: document.getElementById("nombre").value,
          Apellidos: document.getElementById("apellidos").value,
          Edad: document.getElementById("edad").value,
          Id: this.person.Id
        }

        if(data.Id  = -1){
          axios.post('http://10.60.23.16:62270/api/Usuarios/' ,data)
          .then(response => {
            this.person.Id = response.data.Id;
            EventBus.$emit("updateUsuario", this.person);
          })
        }
        else{
          axios.put('http://10.60.23.16:62270/api/Usuarios/' + data.Id, data)
          .then(response => {
            EventBus.$emit("updateUsuario", this.person);
          })
        }
      },

      eliminar: function(){
        this.seen = false;
        axios.delete('http://10.60.23.16:62270/api/Usuarios/' + this.person.Id)
        .then(response => {
          EventBus.$emit("updateUsuario", this.person);
        })
      }
    },
    created() {
      this.person = this.$parent.person;
    }

  }
</script>

<style>
  *{
    padding: 5px;
  }
</style>
