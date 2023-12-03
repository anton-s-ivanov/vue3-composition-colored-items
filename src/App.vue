<script setup>
    import { ref, onMounted } from 'vue';
    import CoreListComponent from './components/CoreListComponent.vue';
    
    const lists = ref([]);
    
    const fillListsObject = (listsNumber) => {
        while(listsNumber) {
            lists.value.push(
                {
                    items: getDefaultItems(getRandomInteger(4,10)),
                    expanded: false,
                    sorted: true
                });
            listsNumber--;
        }
    }

    const getDefaultItems = (itemsNumber) => {
        let items = [];

        while(itemsNumber) {
            items.push(
                {   
                    color: '#' + getRandomInteger(100, 999),
                    qty: getRandomInteger(0, 10),
                    checked: false
                }
            );
            itemsNumber--;
        }

        return items;
    }

    const getRandomInteger = (min, max) => {
        return Math.floor(Math.random() * (max - min) ) + min;
    }

    onMounted(() => {
        fillListsObject(5)
    })

    const setItemQtyValue = (itemData) => {
        if(itemData.qtyValue < 0) {
            return;
        }

        lists.value[itemData.listKey].items[itemData.itemKey].qty = itemData.qtyValue
    }

</script>

<template>
    <div class="container">
        <ul>
            <li v-for="(list, index) in lists" :key="index">
                <core-list-component 
                    :list="list" 
                    :listNum="index + 1" 
                    @setItemQtyValue="setItemQtyValue"
                />
            </li>
        </ul>
    </div>
</template>

<style>
    .container {
        width: 90%;
        padding: 50px;
        margin: auto;
    }

    ul {
        list-style-type: none;
    }

    li {
        line-height: 2
    }

    .pointer {
        cursor: pointer;
    }

</style>