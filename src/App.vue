<template>
  <div class="container-index">
    <div class="error" v-if="error">
      Codigo postal no encontrado
    </div>
    <input maxlength="5" type="text" v-on:input="ctrlInput($event)" v-model="codigoPostal">
    <input :disabled=" codigoPostal.length < 5 ? true : false " type="button" value="Buscar" v-on:click="click()">
    <div v-if="verSelect">
      <select name="Estado">
        <option value="" selected>Seleccione una opción</option>

      </select>
      <select name="municipio">
        <option value="" selected>Seleccione una opción</option>
      </select>
      <select name="colonia">
        <option value="" selected>Seleccione una opción</option>
      </select>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      codigoPostal: '',
      verSelect: false,
      error: false,
      estados: null,
      municipios: null,
      colonias: null
    }
  },
  methods: {
    getCp () {
      axios({ method: 'GET', url: 'http://localhost:3000/cp/' + this.codigoPostal }).then(response => {
        this.ctrlResponse(response)
      }, error => {
        this.error = true
        console.log(error)
        // cambiar a vista de error
      })
    },
    ctrlInput (event) {
      event.target.value = event.target.value.replace(/\D/g, '')
      if (event.target.value.length < 5) {
        this.verSelect = false
      }
    },
    click () {
      this.getCp()
    },
    ctrlResponse (response) {
      if (response.status === 200) {
        this.verSelect = true
        this.buildResponse(response.data.data)
      } else if (response.status === 204) {
        this.error = true
      }
    },
    buildResponse (data) {
      console.log(data)
    }
  }

}
</script>

<style lang="scss">
</style>
