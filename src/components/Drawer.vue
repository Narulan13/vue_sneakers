<script setup>
    import {inject, ref, computed} from 'vue'
    import axios from 'axios';

    import DrawerHead from './DrawerHead.vue'
    import CartItemList from './CartItemList.vue'
    import infoBlock from './infoBlock.vue'

    const isCreating = ref(false);
    const orderId = ref(null)
    const props = defineProps({
        totalPrice: Number,
        vatPrice: Number,
        buttonDisabled: Boolean
    })
    const {
        cart,
    } = inject('cart')

    const ButtonDisabled = computed( () => CarIsEmpty.value || isCreating.value)
    const CarIsEmpty = computed( () => cart.value.length === 0);

    const createOrder = async () => {
    try{
      isCreating.value = true;
      const { data } = await axios.post('https://1a03112a17cc50e1.mokky.dev/orders', {items: cart.value, totalPrice: props.totalPrice.value});
      cart.value = [];

      orderId.value = data.id;
    } catch (error){
      console.log(error)
    } finally {
      isCreating.value = false;
    }
  }
</script>

<template>
    <div class="fixed top-0 left-0 w-full h-full bg-black/70 z-10"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead/>  
        <div v-if="!totalPrice || orderId" class="flex h-full items-center">
            <infoBlock v-if="orderId" title="Заказ оформлен!" imgUrl="/order-success-icon.png" :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"/>
            <infoBlock v-if="!orderId && !totalPrice" title="Корзина пустая" imgUrl="/package-icon.png" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"/>
        </div>
        <CartItemList />

        <div v-if="totalPrice" class="flex flex-col gap-4 my-7">

            <div class="flex gap-2">
                <span>Итого:</span>
                <div class="border-b border-slate-200 w-full border-dotted flex-1"></div>
                <b>{{ totalPrice }} руб.</b>
            </div>

            <div class="flex gap-2">
                <span>Налог 5%:</span>
                <div class="border-b border-slate-200 w-full border-dotted flex-1"></div>
                <b>{{ vatPrice }} руб.</b>
            </div>
            <button @click="createOrder" :disabled = "ButtonDisabled" class="bg-lime-500 text-white py-3 rounded-xl w-full hover:bg-lime-600 transition duration-300 active:bg-lime-700 disabled:bg-slate-300">Оформить заказ</button>
        </div>
    </div>
</template>