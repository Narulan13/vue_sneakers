<script setup>
    import { reactive, watch, inject, ref, onMounted, provide } from 'vue';
    import axios from 'axios';
    import CardList from '../components/CardList.vue';

    const {cart, addToCart, removeFromCart} = inject('cart');

    const items = ref([]);

    const filters = reactive({
        sortBy: 'title',
        searchQuery: ''
    });

    const onChangeSelect = event => {
        filters.sortBy = event.target.value;
    };

    const onChangeSearchInput = event => {
        filters.searchQuery = event.target.value;
    };
    const onClickAddPlus = (item) => {
        if(!item.isAdded){
        addToCart(item)
        } else {
        removeFromCart(item)
        }
    }
    const fetchItems = async () => {
        try {
        const params = {
            sortBy : filters.sortBy
        }
        if(filters.searchQuery){
            params.title = `*${filters.searchQuery}*`
        }
        const { data } = await axios.get(`https://1a03112a17cc50e1.mokky.dev/items`, {
            params
        })

        items.value = data.map((obj) => ({
            ...obj,
            isFavorite : false,
            isAdded : false,
            favoriteId: null
        }))
        } catch (error) {
        console.log(error)
        }
    }

    const fetchFavorite = async () => {
        try {
        const { data: favorites } = await axios.get('https://1a03112a17cc50e1.mokky.dev/favorites')
        items.value = items.value.map(item => {
            const favorite = favorites.find(favorite => favorite.parentId === item.id)
            if(!favorite){
            return item;
            } else {
            return { 
                ...item, 
                isFavorite : true,
                favoriteId:  favorite.id
            };
            }
        })
        } catch (error) {
        console.log(error)
        }
    }
    onMounted( async () => {
        await fetchItems();
        await fetchFavorite();

        items.value = items.value.map((item) => ({
        ...item,
        isAdded : cart.value.some((cartItem) => cartItem.id === item.id)
        }))

        const localCart = localStorage.getItem('cart')
        cart.value = localCart ? JSON.parse(localCart) : []

    })
    watch(cart, () => {
        items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
        }))
    })
    const addToFavorite = async (item) => {
        try{
        if(!item.isFavorite){
            const obj = {
            ...item,
            parentId : item.id
            }
            item.isFavorite = true
            const { data } = await axios.post('https://1a03112a17cc50e1.mokky.dev/favorites', obj)

            item.favoriteId = data.id
        } else {
            item.isFavorite = false
            await axios.delete(`https://1a03112a17cc50e1.mokky.dev/favorites/${item.favoriteId}`)
            item.favoriteId = null  
        }
        } catch (error) {
        console.log(error)
        }
    }
    provide('addToFavorite', addToFavorite)
    watch(filters, fetchItems);

</script>





<template>
    <div v-auto-animate class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="title">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img src="/search.svg" alt="search" class="absolute top-3 left-4">
            <input 
              @input="onChangeSearchInput"
              type="text" placeholder="Поиск..." class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400">
          </div>
        </div>
      </div>
      <div class="mt-10"> 
        <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
      </div>
</template>