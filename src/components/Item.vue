<script setup>

    import { ref, reactive } from 'vue'

    const props = defineProps({
        item: {
            type: Object,
            requried: true
        },
    })

    const editMode = ref(false)


    const item = reactive({
        id: props.item.id,
        name: props.item.name,
        date: props.item.date,
        status: props.item.status,
    })

    const emit = defineEmits([
        'update-middleware'
    ])

    const triggerUpdate = (command) => {
        const newItem = {
            id: item.id,
            name: item.name,
            date: item.date,
            status: item.status,
            command: command
        }

        emit("update-middleware", newItem)

        if(command !== 'delete' || command !== 'complete'){
            toggleEdit()
        }
    }

    const toggleEdit = () => {
        editMode.value = !editMode.value
    }

</script>


<template>
    <div>

        <div v-show="item.status === 'closed'">
            <div class="itemCard">
                <section id="left" class="itemCardElement">{{ item.name }}</section>
                <section id="middle" class="itemCardElement">{{ item.date }}</section>
                <section id="right" class="itemCardElement buttons" >
                    <button id="menuButton" @:click="triggerUpdate('delete')">Delete</button>
                    <button id="menuButton" @:click="triggerUpdate('reopen')">Reopen</button>
                </section>

            </div>
        </div>

        <div v-show="item.status === 'open'">
            <div v-if="!editMode" class="itemCard">
                <section id="left" class="itemCardElement">{{ item.name }}</section>
                <section id="middle" class="itemCardElement">{{ item.date }}</section>
                <section id="right" class="buttons itemCardElement">
                    <button id="menuButton" @:click="toggleEdit">Edit</button>
                    <button id="menuButton" @:click="triggerUpdate('delete')">Delete</button>
                    <button id="menuButton" @:click="triggerUpdate('complete')">Complete</button>
                </section>
            </div>

            <div v-else class="itemCard">
                <input id="left" v-model="item.name"/>
                <input id="middle" v-model="item.date" type="date"  class="itemCardElement"/>
                <section id="right" class="itemCardElement">
                    <input id="right" @:click="toggleEdit" type="button" value="Cancel" />
                    <input id="right" @:click="triggerUpdate" type="button" value="Save"/>
                </section>
            </div>

        </div>

    </div>
</template>