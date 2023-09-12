<script setup>

    defineEmits(['actualizar-cliente', 'eliminar-cliente'])

    defineProps({
        cliente: {
            type: Object,
            required: true
        }
    })

</script>

<template>
    <div class="mx-5 my-10 bg-white shadow-md px-5 py-10 rounded-xl">
        <p class="font-bold mb-3 text-gray-700 uppercase">ID:
            <span class="font-normal normal-case">
                {{ cliente.id }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Empresa:
            <span class="font-normal normal-case">
                {{ cliente.nombre }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Contacto:
            <span class="font-normal normal-case">
                {{ cliente.contacto }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Número:
            <span class="font-normal normal-case">
                {{ cliente.numero }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Correo:
            <span class="font-normal normal-case">
                {{ cliente.email }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Fecha:
            <span class="font-normal normal-case">
                {{ cliente.fecha }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Descripción:
            <span class="font-normal normal-case">
                    {{ cliente.rubro }}
            </span>
        </p>

        <div class="grid md:grid-cols-2  gap-5 mt-10 ">
            <button 
                type="button"
                class="block w-full py-2 px-10 bg-blue-600 hover:bg-blue-700 text-white font-bold uppercase rounded-lg"
                @click="$emit('actualizar-cliente', cliente.id)"    
            >Editar</button>

            <button
                type="button"
                class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg"
                @click="$emit('eliminar-cliente', cliente.id)"    
            >Eliminar</button>
        </div>
    </div>
</template>