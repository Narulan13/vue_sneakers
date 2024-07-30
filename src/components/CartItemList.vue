<script setup>
    import { inject } from 'vue'

    import CartItem from './CartItem.vue'

    const { cart, removeFromCart } = inject('cart');
</script>

<template>
    <div v-auto-animate class="flex flex-col gap-4 flex-1">
        <CartItem @on-click-remove="() => removeFromCart(item)" v-for="item in cart" :key="item.id" :title="item.title" :price="item.price" :imageUrl="item.imageUrl" />
    </div>
</template>