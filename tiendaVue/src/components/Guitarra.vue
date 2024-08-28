<script setup>
 const props = defineProps({
    guitarra: {
        type: Object,
        required: true
    }
 })
 defineEmits(  [
    'agregar-carrito'
 ])
 // Para pasar del padre al hijo debes agregar props al hijo 
 // para pasar informacion del hijo al padre lo haces dentro de la funcion emit "$emit('funcion-delpadre-alHijo', info-delHijo-al-padre)
</script>

<template>
    <div   class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img :alt=" 'imagen'+ guitarra.nombre" class="img-fluid" :src="'/img/' +guitarra.imagen + '.jpg'">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase"> {{  guitarra.nombre  }} </h3>
            <p> {{ guitarra.descripcion }} </p>
            <p class="fw-black text-primary fs-3"> {{ guitarra.precio }} </p>
            <button 
                type="button"
                class="btn btn-dark w-100 "
                @click="$emit('agregar-carrito', 123)"
            >Agregar al Carrito</button>
        </div>
    </div><!-- FIN GUITARRA -->
</template>

<style>

</style>