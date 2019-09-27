<template>
  <div class="container-index">
    <input maxlength="5" type="text" v-on:input="ctrlInput($event)" v-model="codigoPostal">
    <input :disabled=" codigoPostal.length < 5 ? true : false " type="button" value="Buscar" v-on:click="click()">
    <div class="error" v-if="error">
      Codigo postal no encontrado
    </div>
    <div v-if="verSelect">
      <select name="Estado">
        <option value="" selected>Seleccione una opción</option>
        <option v-for="estado in estados" :key="estado" :value="estado">{{estado}}</option>
      </select>
      <select name="municipio">
        <option value="" selected>Seleccione una opción</option>
        <option v-for="municipio in municipios" :key="municipio" :value="municipio">{{municipio}}</option>
      </select>
      <select name="colonia">
        <option value="" selected>Seleccione una opción</option>
        <option v-for="colonia in colonias" :key="colonia" :value="colonia">{{colonia}}</option>
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
      } else if (response.status === 202) {
        this.error = true
      }
    },
    buildResponse (data) {
      const estado = data.map(res => (res.estado))
      const municipio = data.map(res => (res.mnpio))
      const colonias = data.map(res => (res.colonia))
      this.estados = [...new Set(estado)]
      this.municipios = [...new Set(municipio)]
      this.colonias = [...new Set(colonias)]
    }
  }

}
</script>

<style lang="scss">
</style>
