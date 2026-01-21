<script setup>
import { ref, reactive, onMounted,watch } from 'vue';
import {db} from "./data/guitarra"
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';

const guitarras = ref()
const carrito = ref([])
const guitarra = ref({})

watch(carrito, ()=> {
  guardarLocalStorage()
}, {
  deep: true
}) 

onMounted( ()=> {
    guitarras.value = db;
    guitarra.value = db[3]
    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage) {
      carrito.value = JSON.parse(carritoStorage)
    }
})    

const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex( producto =>producto.id == guitarra.id )
    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++
      console.log(carrito.value)
    } else {
      guitarra.cantidad = 1;
      carrito.value.push(guitarra)
      console.log(carrito.value)  
    }
    guardarLocalStorage()
}

// Del padre al hijo se hace con props (agregar-carrito)

const  decrementarCantidad = (id)=> {
  const index = carrito.value.findIndex( producto => producto.id === id )
  if ( carrito.value[index].cantidad <= 1 ) {return}
  carrito.value[index].cantidad--
}

const incrementarCantidad = (id)=> {
  const index = carrito.value.findIndex( producto => producto.id === id )
  if ( carrito.value[index].cantidad >= 5 ) {return}
  carrito.value[index].cantidad++
}

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter( producto =>  producto.id !== id)
}

const vaciarCarrito = ()=> carrito.value = []

const guardarLocalStorage = ()=>  {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

</script>

<template>
<Header
    :carritoCompras="carrito"
    :guitarra="guitarra"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
>

</Header>

<main class="container-xl mt-5">
  <h2 class="text-center">Nuestra Colección</h2>

  <div class="row mt-5">
      <Guitarra 
        v-for="(guitarra, index) in guitarras" 
        :key="index" 
        v-bind:guitarra="guitarra"

        @agregar-carrito="agregarCarrito"
        > 
      </Guitarra>
  </div>
</main>



</template>

<style scoped>

</style>

