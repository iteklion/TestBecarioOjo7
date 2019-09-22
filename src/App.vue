<template>
  <div class="container-index">
    <input maxlength="5" type="text" v-on:input="ctrlInput($event)" v-model="codigoPostal">
    <input :disabled=" codigoPostal.length < 5 ? true : false " type="button" value="Buscar" v-on:click="click()">
    <div v-if="x">
      <select>
        <option value="">Uno</option>
        <option value="">Uno</option>
      </select>
      <select>
        <option value="">dos</option>
        <option value="">Uno</option>
        <option value="">Uno</option>
      </select>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      codigoPostal: '',
      verSelect: false
    }
  },
  methods: {
    ctrlInput (event) {
      event.target.value = event.target.value.replace(/\D/g, '')
      if (event.target.value.length < 5) {
        this.verSelect = false
      }
    },
    click () {
      this.verSelect = true
    }
  }

}
</script>

<style lang="scss">
</style>
