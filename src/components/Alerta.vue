<script setup>
    import { computed } from 'vue'
    const props = defineProps({
        alerta : {
            type: Object,
            required: true
        }
    })

    // Función para inyectar una clase dinámica en caso de ocuparse
    const isError = computed(() => {
        // Si el tipo de alerta es error se retorna un true
        return props.alerta.tipo === 'error'
    })
</script>

<template>
    <div
        :class="[isError ? 'bg-red-500' : 'bg-green-500']"
        class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
    >
        {{ alerta.mensaje }}
    </div>
</template>