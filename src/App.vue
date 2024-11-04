<script setup>
  import { ref, reactive, watch, onMounted } from 'vue'
  import { uid } from 'uid'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

  const pacientes = ref([])

  // Observa cambios en `pacientes` y guarda en `localStorage` cada vez que cambien
  watch(pacientes, () => {
    localStoragePacientes()
  }, {
    deep: true
  })

  // Cargar datos de `localStorage` al montar el componente
  onMounted(() => {
    const pacientesStorage = localStorage.getItem('pacientes')

    // Verificar que `pacientesStorage` tenga un valor válido antes de intentar parsear
    if (pacientesStorage && pacientesStorage !== "undefined") {
      try {
        pacientes.value = JSON.parse(pacientesStorage)
      } catch (error) {
        console.error("Error al parsear JSON:", error)
      }
    }
  })

  const localStoragePacientes = () => {
    // Guardar el valor de `pacientes` en `localStorage`
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

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
    if (paciente.id) {
      const { id } = paciente
      
      const i = pacientes.value.findIndex(paciente => paciente.id === id)

      pacientes.value[i] = {...paciente}
    } else {
      // Crear una copia del paciente y agregarlo al arreglo de pacientes
      pacientes.value.push({
        ...paciente,
        id: uid()
      })
    }

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

  const actualizarPaciente = (id) => {
    // Buscar al paciente que concide con el id enviado, "[0]" hace que retorne solo el primer elemento y lo retorna como objeto
    const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]

    Object.assign(paciente, pacienteEditar)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
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
        :id="paciente.id"
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
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>