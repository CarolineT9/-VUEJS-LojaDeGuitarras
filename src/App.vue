<script setup>
import {ref, reactive, onMounted, watch} from 'vue'
import {db} from '../src/data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitarras = ref(db)
const carrinho =  ref([])
const guitarra =  ref({})

watch(carrinho, () => {
  salvarLocalStorage()
},{
  deep: true

})

onMounted (() =>{
    guitarras.value = db
    guitarra.value = db[4]

    const carrinhoStorage  = localStorage.getItem('carrinho')
    if(carrinhoStorage){
      carrinho.value = JSON.parse(carrinhoStorage)
    }
})

//adicionar guitarra ao carrinho de compras 
const addCar  = (guitarra) =>{
    const existeNoCarrinho = carrinho.value.findIndex(produto => produto.id === guitarra.id)
    if(existeNoCarrinho >= 0 ){
      carrinho.value[existeNoCarrinho].quantidade ++
    }else{
          guitarra.quantidade = 1
          carrinho.value.push(guitarra)
           
    }
    

}
// decrementar quantidde de produtos no carrinho
const decrementarQuantidade = (id) => {
  const index = carrinho.value.findIndex(produto => produto.id === id)
  //validação para evitar números negativos
  if(carrinho.value[index].quantidade <=1) {
    return
  }
  carrinho.value[index].quantidade --
  
}


//incrementar quantidade de produtos no carrinho
const incrementarQuantidade = (id) =>{
  const index = carrinho.value.findIndex(produto => produto.id === id)
  carrinho.value[index].quantidade ++

  
}
// eliminar produtos específicos do carrinho de compras
const eliminarProduto = (id) => {
  carrinho.value = carrinho.value.filter(produto => produto.id !== id)

  
}
// esvaziar o carrinho
const esvaziarCarrinho = () => {
  carrinho.value = []
}

// tornar o carrinho de compras persistente em localstorage

const salvarLocalStorage = () =>{
  localStorage.setItem('carrinho', JSON.stringify(carrinho.value))
}

</script>

<template>
  <div>
    <Header :carrinho="carrinho" :guitarra="guitarra" @add-car="addCar" @decrementar-quantidade="decrementarQuantidade" @incrementar-quantidade="incrementarQuantidade" @eliminar-produto="eliminarProduto" @esvaziar-carrinho="esvaziarCarrinho"/>

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            
            <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @add-car="addCar"/>
            
        </div>
    </main>
    <Footer/>

    
  </div>
  
</template>

