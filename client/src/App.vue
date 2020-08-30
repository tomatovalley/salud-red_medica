<template>
  <div id="app">
    <h1 class="text-center">RedMedica</h1>

    <div>
      <!-- <select v-model="filtro.pais">
        <option value="" disabled selected hidden>Elije el país</option>
        <option value="México">México</option>
      </select>

      <select v-model="filtro.estado">
        <option value="" disabled selected hidden>Elije el estado</option>
        <option value="México">México</option>
        <option>Estados Unidos</option>
      </select> -->

      <select v-model="filtro.ciudad" v-if="ciudades">
        <option value="" disabled selected hidden>Elije la ciudad</option>
        <option
          v-for="ciudad in ciudades"
          :key="ciudad.id"
          :value="ciudad.id"
        >{{ ciudad.nombre }}</option>
      </select>

      <button @click="applySearch">Buscar</button>
      <button @click="removeFilters">Quitar Filtros</button>
    </div>

    <button @click="section = 'Covid19'">Covid19</button>
    <button @click="section = ''">Emergencias</button>
    <table>
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
// Importamos los datos que van a estar en formato json
// import JSONData from './data.json'
import { db } from './db'

console.log(db)


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
    hospitales: [] // Va a guardar los datos de los hospitales para manejarlos dentro del componente
  }),
  firestore: {
    ciudades: db.collection('ciudades'),
    hospitales: db.collection('hospitales')
  },
  watch: {
    section: function(newVal) {
      // Es el handler para cuando cambie el valor del section
      if (newVal === 'Covid19') {
        this.hospitales = this.hospitales.filter(h => h.tipo === newVal)
        return
      }

      // this.hospitales = JSONData.Hospitales
    }
  },
  mounted() {
    // Hace la carga inicial de los datos
    // this.hospitals = JSONData.Hospitales
    // console.log(this.ciudades)
  },
  methods: {
    applySearch() {
      // Hace el manejo de la busqueda y la filtra para ser mostrada
      // if (this.filtro.pais.length) {
      //   this.hospitales = this.hospitales.filter(h => h.Pais === this.filtro.pais)
      // }

      // if (this.filtro.estado.length) {
      //   this.hospitales = this.hospitales.filter(
      //     h => h.Estado === this.filtro.estado
      //   )
      // }

      if (this.filtro.ciudad.length) {
        this.hospitales = this.hospitales.filter(
          h => h.Ciudad === this.filtro.ciudad
        )
      }
    },
    removeFilters() {
      // Reinicia los valores del filtro y carga los datos iniciales de los hospitales
      // this.filtro.pais = ''
      // this.filtro.estado = ''
      this.filtro.ciudad = ''

      // this.hospitals = JSONData.Hospitales
    }
  }
}
</script>

<style>
#app {
  padding: 0 15rem;
}

.text-center {
  text-align: center;
}

table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}

select {
  margin: 0 1rem;
}
</style>
