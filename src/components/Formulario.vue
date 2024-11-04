<script setup>
    import { ref, reactive } from 'vue'

    // Leer datos del formulario con ref
    // const nombre = ref('')

    // Leer datos del formulario con reactive
    const paciente = reactive({
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas: ''
    })

    const validar = () => {
        // Object.values funciona para obtener todos los valores del objeto
        if(Object.values(paciente).includes('')) {

            return
        }
    }
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento de Pacientes</h2>

        <p class="text-lg text-gray-600 mt-5 text-center mb-10">
            Añade Pacientes y 
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <!-- Formulario, v-on para escuchar el evento submit con prevent para prevenir el envío y v-model para leer los datos -->
        <form
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            v-on:submit.prevent="validar"
        >
            <div class="mb-5">
                <label 
                    for="mascota"
                    name="mascota"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre de la Mascota
                </label>
                <!-- v-model para leer los datos -->
                <input 
                    id="mascota"
                    type="text"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-model="paciente.nombre"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="propietario"
                    name="propietario"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre del Propietario
                </label>
                <input 
                    id="mascota"
                    type="text"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-model="paciente.propietario"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="email"
                    name="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                    id="email"
                    type="email"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-model="paciente.email"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="alta"
                    name="alta"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                <input 
                    id="alta"
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-model="paciente.alta"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="sintomas"
                    name="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Síntomas
                </label>
                <textarea
                    id="sintomas"
                    placeholder="Describe los síntomas del paciente"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    v-model="paciente.sintomas"
                />
            </div>

            <input
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                value="Registrar Paciente"
            />
        </form>
    </div>
</template>