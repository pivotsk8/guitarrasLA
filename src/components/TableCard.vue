<script setup>
defineProps({
    producto: {
        type: Object,
        required: true
    }
})

defineEmits([
    'decrementar-cantidad',
    'incrementar-cantidad',
    'eliminar-producto'
])
</script>
<template>
    <td>
        <img class="img-fluid" :src="'/img/' + 'guitarra_02' + '.jpg'" :alt="'producto.nombre' + 'imagen guitarra'">
    </td>
    <td>{{ producto.nombre }}</td>
    <td class="fw-bold">
        {{ producto.precio }}
    </td>
    <td class="flex align-items-start gap-4">
        <button type="button" class="btn btn-dark" @click="$emit('decrementar-cantidad', producto.id)">
            -
        </button>

        {{ producto.cantidad }}

        <button type="button" class="btn btn-dark" @click="$emit('incrementar-cantidad', producto.id)">
            +
        </button>
    </td>
    <td>
        <button class="btn btn-danger" type="button" @click="$emit('eliminar-producto', producto.id)">
            X
        </button>
    </td>
</template>
