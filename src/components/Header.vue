<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="../../public/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="../../public//img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">

                            <p class="text-center" v-if="carrinho.length === 0">El carrito esta vacio</p>
                            <div v-else>
                                <table class="w-100 table" v-if="carrinho.length > 0">
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
                                        <tr v-for="produto in carrinho">
                                            <td>
                                                <img class="img-fluid" :src="'/img/' +produto.imagen + '.jpg'"
                                                    alt="imagen guitarra">
                                            </td>
                                            <td>{{produto.nombre}}</td>
                                            <td class="fw-bold">
                                                {{ produto.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button type="button" class="btn btn-dark" @click="$emit('decrementar-quantidade', produto.id)">
                                                    -
                                                </button>
                                                {{ produto.quantidade }}
                                                <button type="button" class="btn btn-dark" @click="$emit('incrementar-quantidade', produto.id)">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button class="btn btn-danger" type="button"  @click="$emit('eliminar-produto', produto.id)">
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">{{ totalPagar }}</span></p>
                                <button class="btn btn-dark w-100 mt-3 p-2"  @click="$emit('esvaziar-carrinho')">Vaciar Carrito</button>
                            </div>

                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold"> Modelo {{ guitarra.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ guitarra.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">{{ guitarra.precio }}</p>
                    <button type="button" class="btn fs-4 bg-primary text-white py-2 px-5" @click="$emit('add-car', guitarra)">Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="../../public/img/header_guitarra.png" alt="imagen header">
    </header>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    carrinho: {
        type: Array,
        required: true
    },
    guitarra:{
        type: Object,
        required: true 
    }
})
defineEmits(['incrementar-quantidade', 'decrementar-quantidade', 'add-car', 'eliminar-produto', 'esvaziar-carrinho'])

const totalPagar = computed(() =>{
    return props.carrinho.reduce((total, produto) => total + (produto.quantidade * produto.precio), 0)
})

</script>
