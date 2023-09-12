<script setup>
    import { ref, reactive, onMounted, watch } from 'vue'
    import { uid } from 'uid'

    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'
    import Formulario from './components/Formulario.vue'
    import Cliente from './components/Cliente.vue'

    /*ARREGLO PARA LLENAR LA LISTA DE CLIENTES Y AGREGARLOS*/
    const clientes = ref([])

    /*OBJETO DE PACIENTES PARA TOMAR LOS DATOS DEL FORMULARUI*/
    const cliente = reactive({
        id: null,
        nombre: '',
        contacto: '',
        numero: '',
        email: '',
        fecha: '',
        rubro: ''
    })


    /*VA A REESCRIBIR EL STORAGE CUANDO SE AGREGUE, ACTUALICE O ELIMINE UN CLIENTE*/
    watch(clientes, () => {
        guardarLocalStorage()
    }, {
        deep: true
    })

    /*OBTENER CLIENTES ALMACENADOS EN EL LOCAL STORAGE*/
    onMounted(() => {
        const clientesStorage = localStorage.getItem('clientes')
        if (clientesStorage) {
            clientes.value = JSON.parse(clientesStorage)
        }
    })

    /*GUARDAR CLIENTES EN LOCAL STORAGE*/
    const guardarLocalStorage = () => {
        localStorage.setItem('clientes', JSON.stringify(clientes.value))
    }

    const guardarCliente = () => {

        if (cliente.id) {
            const { id } = cliente
            const i = clientes.value.findIndex(cliente => cliente.id === id) //BUSCAR EN EL LISTADO DE CLIENTES UNO QUE TENGA EL MISMO ID
            clientes.value[i] = {...cliente}
        } else {
            clientes.value.push({
                ...cliente, 
                id: uid() //FUNCIÓN PARA GENERA UN ID ÚNICO PARA CADA CLIENTE AGREGADO
            });
        }
        


        //REINICIAR EL OBJETO PRIMERA FORMA
        // cliente.nombre = ''
        // cliente.contacto = ''
        // cliente.numero = ''
        // cliente.email = ''
        // cliente.fecha = ''
        // cliente.rubro = ''

        //REINICIAR EL OBJETO SEGUNDA FORMA
        Object.assign(cliente,{
            nombre: '',
            contacto: '',
            numero: '',
            email: '',
            fecha: '',
            rubro: '',
            id: null
        })
    }

    const actualizarCliente = (id) => {
        const clienteEditar = clientes.value.filter(cliente => cliente.id === id)[0]

        Object.assign(cliente, clienteEditar)
    }

    const eliminarCliente = (id) => {
        clientes.value = clientes.value.filter(cliente => cliente.id !== id)
    }

</script>

<template>
    <div class="container px-4 mx-auto mt-20">
        <Header />

        <div class="mt-12 md:flex">

            <Formulario 
                v-model:nombre="cliente.nombre" 
                v-model:contacto="cliente.contacto" 
                v-model:numero="cliente.numero"
                v-model:email="cliente.email" 
                v-model:fecha="cliente.fecha" 
                v-model:rubro="cliente.rubro"
                @guardar-cliente="guardarCliente" 
                :id="cliente.id"
            />

            <div class="md:w-1/2 md:h-screen overflow-y-auto">
                <h3 class="font-black text-3xl text-center mt-6">Adminsitra tus clientes</h3>

                <p class="text-lg mt-5 text-center mb-10">
                    Listado de 
                    <span class="text-red-600 font-bold">Clientes</span>
                </p>

                <div v-if="clientes.length > 0">

                    <Cliente 
                        v-for="cliente in clientes" 
                        :cliente="cliente" 
                        @actualizar-cliente="actualizarCliente"
                        @eliminar-cliente="eliminarCliente"
                    />

                </div>

                <p v-else class="mt-10 text-2xl text-center">Aún no hay clientes agregados</p>
            </div>

        </div>
    </div>

    <!-- IMPORTAR FOOTER -->
    <Footer />
</template>