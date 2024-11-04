<script setup>
  import { ref, reactive } from 'vue'
  import { uid } from 'uid'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

  const pacientes = ref([])

  // Leer datos del formulario con ref
  // const nombre = ref('')

  // Leer datos del formulario con reactive
  const paciente = reactive({
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
  })

  // Función para guardar paciente
  const guardarPaciente = () => {
    // Crear una copia del paciente y agregarlo al arreglo de pacientes
    pacientes.value.push({
      ...paciente,
      id: uid()
    })

    // Limpiando el formulario
    Object.assign(paciente, {
      id: null,
      nombre: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: ''
    })
  }
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg text-gray-600 mt-5 text-center mb-10">
            Información de los
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>

          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>