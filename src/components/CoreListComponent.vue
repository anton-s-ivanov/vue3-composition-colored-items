<script setup>
    import { computed, defineProps, defineEmits, ref } from 'vue';
    import ItemsListModule from './ItemsListModule.vue';
    import SortedShuffledItemsModule from './SortedShuffledItemsModule.vue';

    const props = defineProps({
        list: Array,
        listNum: Number
    });

    const emit = defineEmits(['setItemQty']);

    let isExpanded = ref(props.list.expanded);

    const masterCheckbox = ref();

    const masterCheckboxIndeterminate = computed(() => {
        const checkedItems = props.list.items.filter(el => el.checked === true);
        if (checkedItems.length < 1) {
            return false;
        }
        return checkedItems.length !== props.list.items.length;
    })

    const setItemsCheckedStatus = (value) => {
        props.list.items.forEach(element => {
            element.checked = value;
        });
    }

    const setMasterCheckboxValue = () => {
        let checkedItems = props.list.items.filter(el => el.checked === true);
        if (!checkedItems?.length)
        masterCheckbox.value.checked = false;

        if (checkedItems?.length === props.list.items.length)
        masterCheckbox.value.checked = true;
    }

    const setItemQtyValue = (itemData) => {
        if(itemData.qtyValue < 0)
        return;
                
        itemData.listKey = props.listNum - 1

        emit('setItemQtyValue', itemData)
    }
</script>

<template>
    <div class="list-title-elems-wrapper">
        <span 
            @click="isExpanded = !isExpanded" class="list-expand-arrow" 
            :class="{'list-expand-arrow-down': isExpanded}"
        >
            &#5171;
        </span>

        <input 
            ref="masterCheckbox" 
            :indeterminate.prop="masterCheckboxIndeterminate" 
            @change="setItemsCheckedStatus($event.target.checked)" 
            type="checkbox" 
            class="select-all-items-checkbox"
        >
        <span @click="isExpanded = !isExpanded" class="list-title">List {{ props.listNum }}</span>
    </div>

    <items-list-module 
        v-if="isExpanded" 
        :list="list" 
        :isExpanded="isExpanded" 
        :masterCheckbox="masterCheckbox" 
        masterCheckboxIndeterminate="masterCheckboxIndeterminate"
        @setMasterCheckboxValue="setMasterCheckboxValue"
        @setItemQtyValue="setItemQtyValue"
    />
    
    <sorted-shuffled-items-module v-if="isExpanded" :list="list" :listNum="listNum" />

</template>

<style>
    .list-title-elems-wrapper {
        display: flex;
        align-items: center;
    }

    .select-all-items-checkbox {
        height: 20px;
        width: 20px;
        margin-right: 10px;
    }

    .list-expand-arrow {
        margin-right: 10px;
        display: inline-block;
        cursor: pointer;
    }

    .list-expand-arrow-down {
        transform: rotate(90deg);
    }

    .item-list-row {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .item-checkbox-title-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .list-title {
        cursor: pointer;
    }

    .item-list-checkbox {
        height: 20px;
        width: 20px;
        margin-right: 20px;
    }

    .item-number-color-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .item-list-number-input {
        height: 20px;
        width: 40px;
        margin-right: 20px;
        padding-left: 5px;
    }

    .item-list-color-input {
        height: 20px;
        width: 20px;
        margin-right: 20px;
        cursor: pointer;
    }

    .item-color-element {
        width: 20px;
        height: 20px;
        border: 1px gray;
    }
</style>