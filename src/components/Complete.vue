<script setup>

    import Item from './Item.vue';

    const props = defineProps({
        closedItems: {
            type: Array,
            required: true
        },
        updateItem: {
            type: Function,
            required: true
        }
    })

    const emit = defineEmits(['update-item'])

    const updateMiddleware = (item) => {
        emit('update-item', item)
    }

</script>



<template>
    <div class="list">
        <h1>Closed</h1>
        <ul v-if="closedItems.length > 0">
            <p v-for="item in closedItems" :key="item.id">
                <Item :item="item" :closedItems="closedItems" @update-middleware="updateMiddleware" />
            </p>
        </ul>
    </div>

</template>