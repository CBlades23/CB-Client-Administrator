<script setup>

    import { reactive, computed } from 'vue'
    import Alerta from './Alerta.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    /*DEFINIENDO LOS EMITS*/
    const emit = defineEmits(['update:nombre', 'update:contacto', 'update:numero', 'update:email', 'update:fecha', 'update:rubro', 'guardar-cliente', 'eliminar-cliente'])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        contacto: {
            type: String,
            required: true
        },
        numero: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        fecha: {
            type: String,
            required: true
        },
        rubro: {
            type: String,
            required: true
        },
    })

    const validar = () => {

        if (Object.values(props).includes('')) {
            alerta.mensaje = 'Todos los campos son obligatorios'
            alerta.tipo = 'error'
            
            setTimeout(() => {
                Object.assign(alerta, {
                    tipo: '',
                    mensaje: ''
                })
            }, 3000);

            return
        }

        emit('guardar-cliente') /*LLAMANDO AL EVENTO guardarCliente*/
        alerta.mensaje = 'Cliente guardado correctamente'
        alerta.tipo = 'exito'
        
        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        }, 3000);
    }

    const editando = computed(() => {
        return props.id
    })

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center mt-6">Formulario de clientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Formulario para
            <span class="text-red-600 font-bold">Añadir clientes</span>
        </p>

        <Alerta v-if="alerta.mensaje" :alerta="alerta" />

        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">

            <div class="mb-5">
                <label for="empresa" class="block text-gray-700 uppercase font-bold">
                    Nombre de empresa
                </label>
                <input type="text" name="empresa" id="empresa" placeholder="Nombre del cliente"
                    class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400" :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                    autocomplete="off">
            </div>

            <div class="mb-5">
                <label for="contacto" class="block text-gray-700 uppercase font-bold">
                    Nombre de contacto
                </label>
                <input type="text" name="contacto" id="contacto" placeholder="Nombre de la persona a contactar"
                    class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400" :value="contacto"
                    @input="$emit('update:contacto', $event.target.value)"
                    autocomplete="off">
            </div>

            <div class="mb-5">
                <label for="numero" class="block text-gray-700 uppercase font-bold">
                    Número
                </label>
                <input type="tel" pattern="[0-9]*" name="numero" id="numero" placeholder="Número de contacto"
                    class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400" :value="numero"
                    @input="$emit('update:numero', $event.target.value)"
                    autocomplete="off">
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Correo
                </label>

                <input type="email" name="email" id="email" placeholder="Email de contacto"
                    class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400" :value="email"
                    @input="$emit('update:email', $event.target.value)"
                    autocomplete="off">
            </div>

            <div class="mb-5">
                <label for="fecha" class="block text-gray-700 uppercase font-bold">
                    Fecha de contacto
                </label>
                <input type="date" name="fecha" id="fecha" class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400"
                    :value="fecha" @input="$emit('update:fecha', $event.target.value)"
                    autocomplete="off">
            </div>

            <div class="mb-5">
                <label for="rubro" class="block text-gray-700 uppercase font-bold">
                    Rubro
                </label>
                <textarea name="rubro" id="rubro"
                    placeholder="Escribe a qué se dedica la empresa y alguna anotación a tener en cuenta"
                    class="border-2 w-full p-2 mt-2 rounded-md placeholder-gray-400 h-40" :value="rubro"
                    @input="$emit('update:rubro', $event.target.value)" />
            </div>

            <input type="submit"
                class="bg-red-600 w-full p-3 text-white uppercase font-bold hover:bg-red-700 cursor-pointer transition-colors"
                :value="[editando ? 'Actualizar Cliente' : 'Registrar Cliente']">

        </form>

</div></template>