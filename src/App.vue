<script setup>
import { ref, onMounted, watch } from 'vue'
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
watch(carrito, () => {
  guardarLocalStorage()
}, { deep: true })

onMounted(() => {
  // 👉 con ref
  guitarras.value = db;
  guitarraPromo.value = db[4]
  const carritoStorage = localStorage.getItem('carrito')
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }


  // 👉 con reactive
  // state.guitarras = db
})

const guardarLocalStorage = () => {
  // ⚠️ no se pueden almacenar arrays en el localStorage tiene que ser un JSON
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

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

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
  carrito.value = []
}
</script>

<template>
  <Header :carrito="carrito" :guitarraPromo="guitarraPromo" @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito" @decrementar-cantidad="decrementarCantidad" @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito" />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <GuitarraCard v-for="guitarra in guitarras" :key="guitarra.id" :guitarra="guitarra"
        @agregar-carrito="agregarCarrito" />
    </div>
  </main>
  <Footer />
</template>

