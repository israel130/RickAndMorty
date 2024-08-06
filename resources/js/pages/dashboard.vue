<template>
  <v-container>

    <div class="d-flex justify-center mb-4">
      <h1>{{ Filtro }}</h1> 
    </div>

    <v-row>
      <v-col cols="12" class="d-flex justify-center mb-4">
        <v-btn
          v-if="pagination.prev"
          @click="Paginacion(pagination.prev)"
          prepend-icon="$vuetify"
          variant="outlined"
          class="ma-2"
        >
          Página Anterior
        </v-btn>
        <v-btn
          v-if="pagination.next"
          @click="Paginacion(pagination.next)"
          prepend-icon="$vuetify"
          variant="outlined"
          class="ma-2"
        > 
          Página Siguiente
        </v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12" class="d-flex justify-center mb-4">
        <v-btn 
          @click="filtrarPorGenero('male')"
          prepend-icon="$vuetify"
          append-icon="$vuetify"
          variant="outlined"
          class="ma-2"
        >
          Hombres
        </v-btn>
        <v-btn 
          @click="filtrarPorGenero('female')"
          prepend-icon="$vuetify"
          append-icon="$vuetify"
          variant="outlined"
          class="ma-2"
        >
          Mujeres
        </v-btn>
        <v-btn 
          @click="obtenerDatos"
          prepend-icon="$vuetify"
          append-icon="$vuetify"
          variant="outlined"
          class="ma-2"
        >
          Todos
        </v-btn>

        <v-text-field
          v-model="nombreFiltro"
          label="Filtro por nombre"
          @input="filtrarPorNombre"
        ></v-text-field>

      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12" md="6" lg="4"  v-for="(info, index) in datos_personas" :key="index">
        <v-card color="#4093db">
          <div class="d-flex flex-no-wrap justify-space-between">
            <div>
              <v-card-title class="text-h5" style="color: white;">
                {{ info.name }}
              </v-card-title>
              <v-card-text>
                <div class="info-container">
                  <span class="d-block mb-2">gender-{{ info.gender }}</span>
                  <span class="d-block mb-2">origin-{{ info.origin.name }}</span>
                  <span class="d-block mb-2">status-{{ info.status }}</span>
                </div>
              </v-card-text>
            </div>
            <v-avatar class="ma-3" rounded="0" size="125">
              <v-img :src="info.image"></v-img>
            </v-avatar>
          </div>
        </v-card>
      </v-col>
    </v-row>

  </v-container>
</template>

<script setup>

  //import Toastify from 'toastify-js'
  import { ref, computed, onMounted } from 'vue';
  import axios from 'axios';
  import { VRow } from 'vuetify/components';

  const datos_personas = ref([]);
  const datosOriginales = ref([]);
  const Filtro = ref('');
  const nombreFiltro = ref('');

  const pagination = ref({
    next: null,
    prev: null
  });

  const obtenerDatos = async () => {
    Filtro.value = 'Todos';
    const response = await axios.get('https://rickandmortyapi.com/api/character');
    datos_personas.value = response.data.results;
    datosOriginales.value = response.data.results;
    pagination.value = {
      next: response.data.info.next,
      prev: response.data.info.prev
    };
    
  };

  const Paginacion = async ( url ) => {
    const response = await axios.get(url);
    datos_personas.value = response.data.results;
    datosOriginales.value = response.data.results; 
    pagination.value = {
      next: response.data.info.next,
      prev: response.data.info.prev
    };
  };

  const filtrarPorNombre = () => {
    if (nombreFiltro.value == '') {
      datos_personas.value = datosOriginales.value;
    } else {
      datos_personas.value = datosOriginales.value.filter(persona =>
        persona.name.toLowerCase().includes(nombreFiltro.value.toLowerCase())
      );
    }
  };


  const filtrarPorGenero = async (genero) => {
    Filtro.value = genero;
    const response = await axios.get(`https://rickandmortyapi.com/api/character?gender=${genero}`);
    datos_personas.value = response.data.results;
    datosOriginales.value = response.data.results; 
    pagination.value = {
      next: response.data.info.next,
      prev: response.data.info.prev
    };
  };

  onMounted(() => {
    obtenerDatos();
  });

</script>

<style scoped>
  .d-flex {
    display: flex;
  }
  .flex-no-wrap {
    flex-wrap: nowrap;
  }
  .justify-center {
    justify-content: center;
  }
  .mb-4 {
    margin-bottom: 1.5rem;
  }
  .info-container {
    display: flex;
    flex-direction: column;
  }
  .d-block {
    display: block;
  }
  .mb-2 {
    margin-bottom: 0.5rem;
  }
</style>
