<script setup>
    import ClienteService from '../services/ClienteService';
    import { FormKit } from '@formkit/vue'
    import { useRoute, useRouter } from 'vue-router';
    import RouterLink from '../components/UI/RouterLink.vue';
    import Heading from '../components/UI/Heading.vue'

    defineProps({
        titulo:{
            type: String
        }
    })

    const router = useRouter()

    const handleSubmit = (data) => {
        data.estado = 1
        ClienteService.agregarCliente(data)
            .then(respuesta => {
                //Redireccionar
                router.push({name:'listado-clientes'})
            })
            .catch(error => console.log(error))
    }
</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="listado-clientes">
                Volver
            </RouterLink>
        </div>
        <Heading>{{ titulo }}</Heading>

        <div class="mx-auto mt-10 bg-white shadow">
            <div class="mx-auto md:w-2/3 py-20 px-6">
                <FormKit
                    type="form"
                    submit-label="Agregar Cliente"
                    incomplete-message="No se pudo enviar, revisa los mensajes"
                    @submit="handleSubmit"
                >
                    <FormKit 
                        type="text"
                        label="Nombre"
                        name="nombre"
                        placeholder="Nombre de Cliente"
                        validation="required"
                        :validation-messages="{required:'El Nombre del Cliente es Obligatorio'}"
                    />

                    <FormKit 
                        type="text"
                        label="Apellido"
                        name="apellido"
                        placeholder="Apellido de Cliente"
                        validation="required"
                        :validation-messages="{required:'El Apellido del Cliente es Obligatorio'}"
                    />

                    <FormKit 
                        type="email"
                        label="Email"
                        name="email"
                        placeholder="Email de Cliente"
                        validation="required|email"
                        :validation-messages="{required:'El Email del Cliente es Obligatorio', email:'Coloca un Email Válido'}"
                    />

                    <FormKit 
                        type="text"
                        label="Télefono"
                        name="telefono"
                        placeholder="Télefono: XXX-XXX-XXXX"
                        validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                        :validation-messages="{matches:'El Formato no es Válido'}"
                    />

                    <FormKit 
                        type="text"
                        label="Empresa"
                        name="empresa"
                        placeholder="Empresa de Cliente"
                    />

                    <FormKit 
                        type="text"
                        label="Puesto"
                        name="puesto"
                        placeholder="Puesto de Cliente"
                    />

                    <!-- 
                        hay que agregar :actions="false" en las props del formulario
                        <FormKit 
                            type="submit"
                            label="Agregar Cliente"
                        /> 
                    -->
                </FormKit>
            </div>
        </div>
    </div>
</template>

<style>
    .formkit-wrapper {
        max-width: 100%;
    }
</style>
