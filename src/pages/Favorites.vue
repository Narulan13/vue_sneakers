<script setup>
    import axios from 'axios'
    import { onMounted, ref } from 'vue';
     
    import CardList from '../components/CardList.vue';

    const favorites = ref([]);

    onMounted( async () => {
        try {
            const {data} = await axios.get('https://1a03112a17cc50e1.mokky.dev/favorites'); 

            favorites.value = data.map((obj) => ({
                ...obj,
                isFavorite: true
            }))
        } catch (err){
            console.log(err)
        }
    })
</script>



<template>
    <h1 class="mb-8 font-bold text-3xl">Мои закладки</h1>
    <CardList :items="favorites" is-Favorites="true" />
</template>