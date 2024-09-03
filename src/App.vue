<script setup>
import {ref, reactive, onMounted} from 'vue'
import {db} from '../src/data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitarras = ref(db)
const carrinho =  ref([])

onMounted (() =>{
    guitarras.value = db
})
const addCar  = (guitarra) =>{
    const existeNoCarrinho = carrinho.value.findIndex(produto.id === guitarra.id)
    guitarra.quantidade = 1
    carrinho.value.push(guitarra)
    return 
}


</script>

<template>
  <div>
    <Header :carrinho="carrinho"/>

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <!-- FIN GUITARRA -->
            <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @add-car="addCar"/>
            
        </div>
    </main>
    <Footer/>

    
  </div>
  
</template>

