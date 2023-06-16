<script setup>
import { ref, reactive, onMounted } from 'vue'
import { db } from './data/guitarras'
import GuitarraCard from './components/GuitarraCard.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
// 👉 con ref
const guitarras = ref([])
const carrito = ref([])
const guitarraPromo = ref({})

// 👉 con reactive
// const state = reactive({
//   guitarras: []
// })

onMounted(() => {
  // 👉 con ref
  guitarras.value = db;
  guitarraPromo.value = db[4]

  // 👉 con reactive
  // state.guitarras = db
})

const agregarCarrito = (guitarra) => {
  const productInCart = carrito.value.findIndex(producto => producto.id === guitarra.id)
  productInCart >= 0
    ? carrito.value[productInCart].cantidad++
    : (guitarra.cantidad = 1,
      carrito.value.push(guitarra));
}

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  carrito.value[index].cantidad++
}

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if (carrito.value[index].cantidad <= 1) return
  carrito.value[index].cantidad--
}
</script>

<template>
  <Header :carrito="carrito" :guitarraPromo="guitarraPromo" @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito" @decrementar-cantidad="decrementarCantidad" />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <GuitarraCard v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito" />
    </div>
  </main>
  <Footer />
</template>

