<script setup>
    defineProps({
        title: String,
        imgUrl: String,
        description: String
    })
</script>



<template>
    <div class="flex flex-col text-center items-center w-72 mx-auto">
        <img :src="imgUrl" height="80px" width="80px">
        <h2 class="mt-4 font-medium text-2xl">{{ title }}</h2>
        <p class="text-slate-400 mt-2">{{ description }}</p>
    </div>
</template>

