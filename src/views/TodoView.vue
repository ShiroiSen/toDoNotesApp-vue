<script>
import { ref } from 'vue'

export default {
  setup() {
    const dataTodo = ref({
      title: '',
      ToDo: '',
      date: '',
      time: '',
      priority: 'deferred',
      category: ''
    })

    const savedDataArray = ref([])
    const editIndex = ref(null)

    const addItem = () => {
        const TodoData = JSON.parse(localStorage.getItem('ArrayToDos')) || []
        if (editIndex.value !== null) {
            TodoData[editIndex.value] = { ...dataTodo.value }
            editIndex.value = null
        } else {
            TodoData.unshift({ ...dataTodo.value })
        }
        localStorage.setItem('ArrayToDos', JSON.stringify(TodoData))
        savedDataArray.value = TodoData
        dataTodo.value = { title: '', ToDo: '', date: '', time: '', priority: 'deferred', category: '' }

    }

    const editItem = (index) => {
        dataTodo.value = { ...savedDataArray.value[index] }
        editIndex.value = index
    }

    const deleteItem = (index) => {
        const TodoData = JSON.parse(localStorage.getItem('ArrayToDos')) || []
        TodoData.splice(index, 1)
        localStorage.setItem('ArrayToDos', JSON.stringify(TodoData))
        savedDataArray.value = TodoData
    }

    if (localStorage.getItem('ArrayToDos')) {
        savedDataArray.value = JSON.parse(localStorage.getItem('ArrayToDos'))
    }
    
    const truncatedToDo = (toDoText) => {
      const limitToDo = 85
      return toDoText.length > limitToDo ? toDoText.slice(0, limitToDo) + "..." : toDoText
    }

    const getPriorityClass = (priority) => {
      switch (priority) {
        case 'important':
            return 'bg-red-500 text-white rounded-full px-2.5';
        case 'urgent':
            return 'bg-yellow-500 text-white rounded-full px-2.5';
        case 'deferred':
            return 'bg-green-500 text-white rounded-full px-2.5';
        default:
            return '';
      }
    };

    return {
        dataTodo,
        savedDataArray,
        addItem,
        editItem,
        deleteItem,
        truncatedToDo,
        getPriorityClass,
    }
  },
}
</script>

<template>
    <div class="max-w-xl mx-auto flex flex-col justify-center border border-gray-500 rounded-lg p-6 mt-6">
        <form @submit.prevent="addItem">
            <div class="my-6">
                <label for="title" class="block mb-2 text-sm font-medium text-gray-900">Title</label>
                <input type="text" id="title" v-model="dataTodo.title" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg block w-full p-2.5" required>
            </div>
            <div class="mb-6">
                <label for="ToDo" class="block mb-2 text-sm font-medium text-gray-900">To Do</label>
                <textarea type="text" id="ToDo" v-model="dataTodo.ToDo" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg block w-full p-2.5"></textarea>
            </div>
            <div class="grid gap-x-6 mb-6 grid-cols-2">
                <label for="date" class="block mb-2 text-sm font-medium text-gray-900">Date (optional)</label>
                <label for="time" class="block mb-2 text-sm font-medium text-gray-900">Time (optional)</label>
                <div>
                    <input id="date" type="date" v-model="dataTodo.date" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg block w-full p-2.5" placeholder="John" />
                </div>
                <div>
                    <input id="time" type="time" v-model="dataTodo.time" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg block w-full p-2.5 "/>
                </div>
            </div>
            <div class="mb-6">
                <label class="block mb-2 text-sm font-medium text-gray-900">Priority</label>
                <div class="grid grid-cols-3 gap-6">
                    <input type="radio" id="important" v-model="dataTodo.priority" name="priority." value="important" class="hidden peer/important justify-center">
                    <label for="important" class="peer-checked/important:text-white peer-checked/important:bg-red-500 border rounded-lg border-gray-500 block w-full p-2.5 font-medium text-center">Important</label>
                    <input type="radio" id="urgent" v-model="dataTodo.priority" name="priority." value="urgent" class="hidden peer/urgent">
                    <label for="urgent" class="peer-checked/urgent:text-white peer-checked/urgent:bg-yellow-500 border rounded-lg border-gray-500 block w-full p-2.5 font-medium text-center">Urgent</label>
                    <input type="radio" id="deferred" v-model="dataTodo.priority" name="priority." value="deferred" class="hidden peer/deferred" checked>
                    <label for="deferred" class="peer-checked/deferred:text-white peer-checked/deferred:bg-green-500 border rounded-lg border-gray-500 block w-full p-2.5 font-medium text-center">Deferred</label>
                </div>
            </div>
            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none font-medium rounded-lg text-sm w-auto px-5 py-2.5 text-center">
                Save
            </button>
        </form>
    </div>
    <div class="max-w-4xl mx-auto justify-center mt-6 flex flex-wrap gap-6">
        <div v-for="(data, index) in savedDataArray" :key="index" class="border border-gray-500 rounded-lg p-2.5 inline-block w-fit h-fit max-w-md">
            <p class="mb-2 capitalize font-bold text-xl">{{ data.title }}<span class="font-thin ml-2" :class="getPriorityClass(data.priority)">{{ data.priority }}</span></p>
            <div class="grid grid-cols-2 mb-2">
                <p>Date: {{ data.date || '-' }} </p>
                <p>Time: {{ data.time || '-' }}</p>
            </div>
            <div class="bg-gray-200 p-2.5 rounded-lg mb-2">
                <p>{{ truncatedToDo(data.ToDo) }}</p>
            </div>
            <div class="flex justify-end gap-3">
                <button @click="editItem(index)" class="border text-yellow-500 hover:bg-yellow-500 hover:text-white font-medium text-center px-2.5 py-1.5 rounded-lg">edit</button>
                <button @click="deleteItem(index)" class="border text-red-500 hover:bg-red-500 hover:text-white font-medium text-center px-2.5 py-1.5 rounded-lg">delete</button>
            </div>
        </div>
    </div>
</template>
