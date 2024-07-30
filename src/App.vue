<script setup>
  import { ref, provide, computed ,watch } from 'vue';


  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';

  const cart = ref([])

  const totalPrice = computed( () => cart.value.reduce((acc, item) => acc + item.price, 0))
  const vatPrice = computed( () => Math.round((totalPrice.value * 5)/100))


  const drawerOpen = ref(false)

  const closeDrawer = () => {
    drawerOpen.value = false
  }
  const openDrawer = () => {
    drawerOpen.value = true
  }

  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded=true
  }
  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }


  

  watch(cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  }, {deep: true})

  provide ('cart', {cart, closeDrawer, openDrawer, addToCart, removeFromCart} );
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice" @create-order="createOrder" :button-disabled="cartButtonDisabled"/>
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header @open-drawer="openDrawer" :total-price="totalPrice" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped>
</style>
