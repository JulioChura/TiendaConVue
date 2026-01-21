<script setup>

import { computed } from 'vue';

const props = defineProps({
    carritoCompras: {
        type: Array,
        required: true
    },
    guitarra: {
        type: Object,
        required: true
    }
})

defineEmits([
    'incrementar-cantidad',
    'decrementar-cantidad',
    'agregar-carrito',
    'vaciar-carrito'
])

const totalPagar = computed( ()=> {
    return props.carritoCompras.reduce( (total, producto) => (producto.cantidad*producto.precio), 0)
})

</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="carritoCompras.length === 0" class="text-center">El carrito esta vacio</p>

                            <div v-else>
                                <table v-if="carritoCompras.length !== 0" class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr
                                            v-for="(producto, index) in carritoCompras" in :key="index">
                                            <td>
                                                <img class="img-fluid" :src="'/img/'+producto.imagen + '.jpg'"
                                                    :alt="'imagen guitarra' + guitarra">
                                            </td>
                                            <td> {{ producto.nombre }} </td>
                                            <td class="fw-bold">
                                                {{ producto.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button @click="$emit('decrementar-cantidad', producto.id)" type="button" class="btn btn-dark">
                                                    -
                                                </button>
                                                {{ producto.cantidad  }}
                                                <button @click="$emit('incrementar-cantidad', producto.id)" type="button" class="btn btn-dark">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button @click="$emit('eliminar-producto', producto.id)" class="btn btn-danger" type="button">
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p v-if="carritoCompras.length > 0" class="text-end">Total pagar: <span
                                        class="fw-bold">{{ totalPagar }}</span></p>
                                <button @click="$emit('vaciar-carrito')" class="btn btn-dark w-100 mt-3 p-2">Vaciar Carrito</button>
                            </div>

                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Guitarra {{ guitarra.nombre }} </h1>
                    <p class="mt-5 fs-5 text-white"> {{ guitarra.descripcion }} </p>
                    <p class="text-primary fs-1 fw-black"> {{ guitarra.precio }} </p>
                    <button  @click="$emit('agregar-carrito', guitarra)" type="button" class="btn fs-4 bg-primary text-white py-2 px-5">Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>

<style></style>