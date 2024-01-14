<template>
  <div class="container mx-auto p-4">
    <div class="items-center justify-center h-screen" v-if="!mostrarEncuesta">
      <h1 class="text-3xl font-bold mb-6">Para obtener tu regalo de 60 cumpleaños, vas a rellenar una pequeña encuesta con el fin de personalizar más aún el presente.</h1>
      <h2 class="text-xl font-bold mb-6">Pulsa el botón para empezar</h2>
      <button 
        class="px-4 py-2 text-white font-bold rounded bg-teal-800 hover:bg-teal-700" 
        @click="mostrarEncuesta = true">
        Empezar
      </button>
    </div>
    <div v-if="mostrarEncuesta && !mensajeEnviado">
      <h1 class="text-3xl font-bold mb-6">Encuesta</h1>
      <form @submit.prevent="enviarRespuestas">
        <!-- Pregunta 1 -->
        <div class="mb-4">
          <h2 class="text-xl font-bold mb-2">Preferencias Culturales</h2>
          <div v-for="(opcion, ciudad) in Object.entries(opcionesPregunta1)" :key="opcion" class="mb-2">
            <label class="inline-flex items-center">
              <input type="radio" class="form-radio text-teal-800 checked:bg-teal-700" :value="opcion[0]" v-model="respuestaPregunta1">
              <span class="ml-2">{{ opcion[1] }}</span>
            </label>
          </div>
        </div>

        <!-- Pregunta 2 -->
        <div class="mb-4">
          <h2 class="text-xl font-bold mb-2">Actividades Preferidas</h2>
          <div v-for="(opcion, ciudad) in Object.entries(opcionesPregunta2)" :key="opcion" class="mb-2">
            <label class="inline-flex items-center">
              <input type="radio" class="form-radio text-teal-800 checked:bg-teal-700" :value="opcion[0]" v-model="respuestaPregunta2">
              <span class="ml-2">{{ opcion[1] }}</span>
            </label>
          </div>
        </div>

        <!-- Pregunta 3 -->
        <div class="mb-4">
          <h2 class="text-xl font-bold mb-2">Tipo de Gastronomía</h2>
          <div v-for="(opcion, ciudad) in Object.entries(opcionesPregunta3)" :key="opcion" class="mb-2">
            <label class="inline-flex items-center">
              <input type="radio" class="form-radio text-teal-800 checked:bg-teal-700" :value="opcion[0]" v-model="respuestaPregunta3">
              <span class="ml-2">{{ opcion[1] }}</span>
            </label>
          </div>
        </div>

        <!-- Pregunta 4 -->
        <div class="mb-6">
          <h2 class="text-xl font-bold mb-2">Tipo de eventos históricos</h2>
          <div v-for="(opcion, ciudad) in Object.entries(opcionesPregunta4)" :key="opcion" class="mb-2">
            <label class="inline-flex items-center">
              <input type="radio" class="form-radio text-teal-800 checked:bg-teal-700" :value="opcion[0]" v-model="respuestaPregunta4">
              <span class="ml-2">{{ opcion[1] }}</span>
            </label>
          </div>
        </div>

        <!-- Botón Enviar -->
        <button 
          type="submit" 
          class="px-4 py-2 text-white font-bold rounded bg-teal-800 hover:bg-teal-700" 
          :v-if="!formularioValido">
          Enviar
        </button>
      </form>
    </div>

    <!-- Mensaje de confirmación -->
  </div>
  <div class="items-center justify-center h-screen" v-if="mensajeEnviado">
    <h1 class="text-2xl mb-6">{{ mensajeEnviado }}</h1>
    <h1 class="text-4xl font-bold mb-6">{{ ciudadConMasPuntos }}</h1>
    <h1 class="text-2xl mb-6">{{ submensajeEnviado }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      opcionesPregunta1: {
          Praga: 'Arquitectura medieval',
          Dublin: 'Calles empedradas',
          Budapest: 'Arquitectura del Imperio Austrohúngaro',
          Liubliana: 'Ciudades en la montaña'
      },
      opcionesPregunta2: {
          Praga: 'Visitas a castillos',
          Dublin: 'Música en vivo',
          Budapest: 'Cruceros por el río',
          Liubliana: 'Paseos en la naturaleza'
      },
      opcionesPregunta3: {
          Praga: 'Platos de carne',
          Dublin: 'Cerveza negra',
          Budapest: 'Estofados',
          Liubliana: 'Influencias italianas'
      },
      opcionesPregunta4: {
          Praga: 'Revoluciones',
          Dublin: 'Luchas por la independencia',
          Budapest: 'Imperios',
          Liubliana: 'Zonas de conflicto'
      },
      respuestaPregunta1: '',
      respuestaPregunta2: '',
      respuestaPregunta3: '',
      respuestaPregunta4: '',
      mensajeEnviado: '',
      submensajeEnviado: '',
      ciudadConMasPuntos: '',
      mostrarEncuesta: false
    };
  },
  mounted() {
    this.aleatorio();
  },
  computed: {
    formularioValido() {
      return this.respuestaPregunta1 && this.respuestaPregunta2 && this.respuestaPregunta3 && this.respuestaPregunta4;
    }
  },
  methods: {
    aleatorio() {
      this.opcionesPregunta1 = this.mezclarClaves(this.opcionesPregunta1);
      this.opcionesPregunta2 = this.mezclarClaves(this.opcionesPregunta2);
      this.opcionesPregunta3 = this.mezclarClaves(this.opcionesPregunta3);
      this.opcionesPregunta4 = this.mezclarClaves(this.opcionesPregunta4);
    },
    mezclarClaves(objeto) {
      let entradas = Object.entries(objeto);
      // Algoritmo de Fisher-Yates para mezclar un array
      for (let i = entradas.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [entradas[i], entradas[j]] = [entradas[j], entradas[i]]; // Intercambiar elementos
      }
      return Object.fromEntries(entradas); // Devuelve el array mezclado
      // Para obtener un objeto: return Object.fromEntries(entradas);
    },
    enviarRespuestas() {
      let ciudades = {
        Praga: 0,
        Dublin: 0,
        Budapest: 0,
        Liubliana: 0
      }
      
      ciudades[this.respuestaPregunta1]++;
      ciudades[this.respuestaPregunta2]++;
      ciudades[this.respuestaPregunta3]++;
      ciudades[this.respuestaPregunta4]++;

      let ciudadConMasPuntos = '';
      for (let ciudad in ciudades) {
        if (ciudadConMasPuntos === '' || ciudades[ciudad] >= ciudades[ciudadConMasPuntos]) {
          ciudadConMasPuntos = ciudad;
        }
      }
      this.mensajeEnviado = `Tu regalo es un viaje a:`;
      this.ciudadConMasPuntos = ciudadConMasPuntos;
      this.submensajeEnviado = `acompañado por Héctor y Miguel. ¡Felicidades!`;
    }
  }
};
</script>

<style>
.pregunta {
  margin-bottom: 20px;
}

.opciones label {
  display: block;
  margin-bottom: 5px;
}
</style>
