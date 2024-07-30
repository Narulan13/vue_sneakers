<script setup>
    defineProps({
        id: Number,
        isFavorite: Boolean,
        imageUrl: String,
        title: String,
        price: Number,
        isAdded: Boolean,
        onClickAdd: Function,
        onClickFavorite: Function
    });
</script>



<template>
    <div class="relative border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition duration-200">
        <img v-if="onClickFavorite" :src=" !isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="like" class="absolute top-8 left-8" @click="onClickFavorite">
        <img :src="imageUrl" alt="sneaker">
        <p class="mt-2">{{ title }}</p>
        <div class="flex justify-between">
            <div class="flex flex-col">
            <span class="text-slate-400">Цена:</span>
            <b>{{ price }} руб.</b>
            </div>
            <img v-if="onClickAdd" :src="!isAdded ? '/plus.svg' : 'checked.svg'" alt="add-to-cart" @click="onClickAdd">
        </div>
    </div>
</template>