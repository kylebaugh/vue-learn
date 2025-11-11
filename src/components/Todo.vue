<script setup>
    import Item from './Item.vue';

    const props = defineProps({
        openItems: {
            type: Array,
            required: true
        },
        updateItem:{
            type: Function,
            required: true
        }
    })

    const emit = defineEmits(['update-item'])

    const updateMiddleware = (item) => {
        emit("update-item", item)
    }

</script>



<template>
    <div class="list">
        <h1>Open</h1>
        <ul v-if="openItems.length > 0">
            <p v-for="item in openItems" :key="item.id">
                <Item :item="item" :openItems="openItems" @update-middleware="updateMiddleware"/>
            </p>
        </ul>

    </div>
</template>