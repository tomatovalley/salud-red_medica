<template>
  <div id="app" class="container">
    <h1 class="text-center">RedMedica</h1>
    <div> 
      <select v-model="filtro.ciudad" v-if="ciudades" class="form-control d-inline-block mr-4" style="width: 200px">
        <option value="" disabled selected hidden>Elije la ciudad</option>
        <option
          v-for="ciudad in ciudades"
          :key="ciudad.id"
          :value="ciudad.id"
        >{{ ciudad.nombre }}</option>
      </select>

      <button class="btn btn-outline-primary mr-2" type="button" @click="applySearch">Buscar </button>
      <button class="btn btn-outline-secondary" type="button" @click="removeFilters">Quitar Filtros </button> 
    </div>
      <br>
    <button class="btn btn-danger" type="button" @click="section = 'Covid19'">Covid19 </button>
    <button class="btn btn-info" type="button" @click="section = 'Emergencias'">Emergencias </button>
    <table class="table">
      <thead>
        <tr>
          <th>Hospital</th>
          <th>Cupo</th>
          <th>Dirección</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="hospital in hospitales" :key="hospital.id">
          <td>{{ hospital.nombre }}</td>
          <td>{{ hospital.cupos_disponibles }}</td>
          <td>
            <a
              :href="`https://www.google.com/maps/search/${hospital.direccion}`"
              target="_blank"
            >
              {{ hospital.direccion }}
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { db } from './db'

// Estructura de nuestro componente
export default {
  data: () => ({
    filtro: {
      // Va a guardar los valores del filtro
      pais: '',
      estado: '',
      ciudad: '',
      ciudades: []
    },
    section: '', // Se encarga de guardar la seccion de la busqueda 'Covid19' ó 'Emergencias'
    hospitales: [], // Va a guardar los datos de los hospitales para manejarlos dentro del componente
    ciudades: []
  }),
  watch: {
    section: function(newVal) {
      // Es el handler para cuando cambie el valor del section
      if (newVal === 'Covid19') {
        this.hospitales = this.hospitales.filter(h => h.tipo === newVal)
        return
      }
      if(newVal === 'Emergencias') {
        this.hospitales = this.hospitales.filter(h => h.tipo === newVal)
        return
      }
      this.hospitales = db.collection('hospitales')
    }
  },
  mounted() {
    this.$bind('ciudades', db.collection('ciudades'))
    this.$bind('hospitales', db.collection('hospitales'))
  },
  methods: {
    applySearch() {
      if (this.filtro.ciudad.length) {
        this.hospitales = this.hospitales.filter(
          h => h.ciudad_id === this.filtro.ciudad
        )
      }
    },
    removeFilters() {
      // Reinicia los valores del filtro y carga los datos iniciales de los hospitales
      this.filtro.ciudad = ''

      this.$bind('hospitales', db.collection('hospitales'))
    }
  }

  
}
</script>