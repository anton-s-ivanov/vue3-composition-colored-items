<script setup>
    import { defineProps, ref } from 'vue';

    const props = defineProps({
        list: Array,
        listNum: Number
    });

    let sorted = ref(true);

    let shuffledItems = () => {
        let mergedItems = [];
        props.list.items.forEach(el=>{
            let newItems = new Array(el.qty).fill({color: el.color});
            mergedItems = mergedItems.concat(newItems);
        })

        return suffleArray(mergedItems);
    }

    const suffleArray = (array) => {
        return array.sort(() => Math.random() - 0.5);
    }
</script>

<template>
    <div class="sorted-shuffled-items-wrapper">
        <div class="title-button-wrapper">
            <div class="title">List {{ props.listNum }}</div>
            <input 
                type="button" 
                @click="sorted = !sorted" 
                class="sort-shuffle-button" 
                :value="sorted ? 'Перемешать' : 'Сортировать'" 
            />
        </div>
        
        <div v-if="sorted" class="item-color-elements-rows-wrapper">
            <div 
                v-for="(item, index) in props.list.items" 
                :key="index" 
                class="item-color-elements-row"
            >
                <div 
                    v-for="(itemElement, index) in new Array(item.qty)" 
                    :key="index" class="item-color-element ml-5 mb-5" 
                    :style="{background: item.color }"
                    ></div>
            </div>
        </div>
       
        <div v-if="!sorted" class="item-color-elements-shuffled-wrapper">
            <div 
                v-for="(itemElement, index) in shuffledItems()" 
                :key="index" class="item-color-element ml-5 mb-5" 
                :style="{background: itemElement.color }"
            ></div>
        </div>
    </div>
</template>

<style scoped>
    .sorted-shuffled-items-wrapper {
        margin-top: 20px;
        margin-bottom: 20px;
        border: 1px solid gray;
        width: 90%;
        margin-left: auto;
        padding: 10px;
    }
    .title {
        margin-bottom: 10px;
    }

    .ml-5 {
        margin-right: 5px;
    }

    .mb-5 {
        margin-bottom: 5px;
    }

    .item-color-elements-row {
        display: flex;
        flex-wrap: wrap;
    }

    .item-color-elements-shuffled-wrapper {
        display: flex;
        flex-wrap: wrap;
    }

    .title-button-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 100%;
    }

    .sort-shuffle-button {
        cursor: pointer;
        background: #42b2ff;
        color: white;
        font-weight: bold;
        border-radius: 5px;
        border: 0;
        box-shadow: 0px 0px 0px 2px #cbd9e8;
    }
</style>