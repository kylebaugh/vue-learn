<script setup>
import {ref, reactive} from 'vue'
import Todo from './components/Todo.vue';
import Complete from './components/Complete.vue';


const openItems = ref([
  {id: 1, name: 'Dishes', date: '2025-11-15', status: 'open'},
  {id: 3, name: 'Sweep', date: '2025-11-20', status: 'open'},
])
const closedItems = ref([
  {id: 2, name: 'Laundry', date: '2025-11-11', status: 'closed'},
  {id: 4, name: 'Mop', date: '2025-11-17', status: 'closed'},
])

const id = ref(5)
const formOpen = ref(false)

const newItem = reactive({
  id:id,
  name: '',
  date: '',
})

const addNewItem = () => {
  const item = {
    id: id.value,
    name: newItem.name,
    date: newItem.date,
    status: 'open'
  }

  openItems.value.push(item)

  id.value += 1

  newItem.name = ''
  newItem.date = ''
}

const toggleForm = () => {
  if(formOpen.value){
    newItem.name = ''
    newItem.date = ''
  }

  formOpen.value = !formOpen.value
}

const updateOpenItem = (updatedItem) => {

  if(updatedItem.command === 'delete'){
      if(updatedItem.status === 'closed'){
        const index = closedItems.value.findIndex((el) => el.id === updatedItem.id)
        closedItems.value.splice(index, 1)
      }else{
        const index = openItems.value.findIndex((el) => el.id === updatedItem.id)
        openItems.value.splice(index, 1)
      }

  } else if (updatedItem.command === 'complete'){
      const index = openItems.value.findIndex((el) => el.id === updatedItem.id)
      closedItems.value.push({...openItems.value[index], status: 'closed'})
      openItems.value.splice(index, 1)

    } else if (updatedItem.command === 'reopen'){
      const index = closedItems.value.findIndex((el) => el.id === updatedItem.id)
      openItems.value.push({...closedItems.value[index], status: 'open'})
      closedItems.value.splice(index, 1)

    } else {
      const index = openItems.value.findIndex((el) => el.id === updatedItem.id)
      openItems.value.splice(index, 1, updatedItem)
  }

}

</script>

<template>
  <div id="home">
    <h1>To-Do Listerino!</h1>

    <button @:click="toggleForm" v-if="!formOpen">Add Item</button>

    <div class="formDiv" v-if="formOpen">

      <form @submit.prevent="addNewItem" v-if="formOpen" class="itemForm">
        <label for="name" class="formLabel">Task Name:</label>
        <input type="text" id="name" v-model="newItem.name" class="formLabel">

        <label for="date">Due Date:</label>
        <input type="date" name="date" id="date" v-model="newItem.date" class="formLabel">

      </form>

      <section id="addFormButtons">
        <button @:click="toggleForm" class="formButton">Cancel</button>
        <button @:click="addNewItem" class="formButton">Submit</button>
      </section>
    </div>

    <div id="listDisplay">
      <Todo @update-item="updateOpenItem" :openItems="openItems"></Todo>
      <Complete @update-item="updateOpenItem" :closedItems="closedItems"></Complete>
    </div>

  </div>
</template>

<style scoped></style>
