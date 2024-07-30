<script setup>
    const emit = defineEmits(['on-click-remove'])

    defineProps ({
        id:Number,
        title: String,
        imageUrl: String,
        price: Number,
        isFavorite: Boolean,
        isAdded: Boolean,
        onClickFavorite: Function,
        onClickAdd: Function 
    })
</script>

<template>
    <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
        <img class="w-16 h-16" :src='imageUrl' alt="sneaker">
        <div class="flex flex-col flex-1">
            <p>{{ title }}</p>
            <div class="flex justify-between mt-2">
                <b class="flex-1">{{ price }} руб.</b>
                <img @click = "() => emit('on-click-remove')" class="opacity-30 hover:opacity-100 transiton duration-300 cursor-pointer" src="/close.svg" alt="">
            </div>
        </div>
    </div>
</template>