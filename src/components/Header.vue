<script setup>
import TableCard from './TableCard.vue';
import PromoGuitar from './PromoGuitar.vue';
import { computed } from 'vue'

const props = defineProps({
    carrito: {
        type: Array,
        required: true
    },
    guitarraPromo: {
        type: Object,
        required: true
    }
})

defineEmits([
    'decrementar-cantidad',
    'incrementar-cantidad',
    'agregar-carrito'
])

const totalPagar = computed(() => {
    return props.carrito.reduce(
        (total, producto) => total + (
            producto.cantidad * producto.precio
        ), 0
    )
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
                            <p v-if="carrito.length === 0" class="text-center m-0">
                                El carrito esta vacio
                            </p>
                            <div v-else>
                                <table class="w-100 table">
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
                                        <tr v-for="producto in  carrito ">
                                            <TableCard :producto="producto"
                                                @incrementar-cantidad="$emit('incrementarCantidad', $event)"
                                                @decrementar-cantidad="$emit('decrementarCantidad', $event)" />
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">${{ totalPagar }}</span></p>
                                <button class="btn btn-dark w-100 mt-3 p-2">Vaciar Carrito</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->
            <PromoGuitar :guitarraPromo="guitarraPromo" @agregar-carrito="$emit('agregar-carrito', $event)" />
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>
