<script setup>
    import { defineProps, defineEmits } from 'vue';

    const props = defineProps({
        list: Array,
        isExpanded: Boolean,
        masterCheckboxIndeterminate: Boolean
    });

    const emit = defineEmits(['setItemQty']);
        
    const setItemQtyValue = (element, itemKey) => {
        let newValue = 0;
        if(Number(element.value)>=0) {
            newValue = Number(element.value);
        } 
        element.value = newValue;
        const itemData = {
            qtyValue: newValue,
            itemKey: itemKey
        }
        emit('setItemQtyValue', itemData)
    }
</script>

<template>
    <ul>
        <li v-for="(item, index) in props.list.items" :key="index" >
            <div class="item-list-row">
                <div class="item-checkbox-title-wrapper">
                    <input 
                        @change="$emit('setMasterCheckboxValue')" 
                        class="item-list-checkbox" 
                        type="checkbox" 
                        v-model="item.checked"
                    >
                    Item {{ (index + 1) }}
                </div>
                <div v-if="item.checked" class="item-number-color-wrapper">
                    <input class="item-list-color-input" type="color" v-model="item.color">
                    <input                     
                        @change="setItemQtyValue($event.target, index)"
                        class="item-list-number-input" 
                        type="number" 
                        min="0" 
                        :value="item.qty"
                    >
                    <div 
                        @click="item.qty > 0 ? item.qty-- : 
                        item.qty = 0" 
                        class="item-color-element pointer" 
                        :style="{background: item.color }"
                    ></div>
                </div>
            </div>
        </li>
    </ul>
</template>