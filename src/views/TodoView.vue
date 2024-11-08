<script>
import { ref } from 'vue'

export default {
  setup() {
    const dataTodo = ref({
      title: '',
      ToDo: '',
      date: '',
      time: '',
      priority: '',
      category: ''
    })

    const savedDataArray = ref([])
    const editIndex = ref(null);

    const addDataToLocalStorage = () => {
        const TodoData = JSON.parse(localStorage.getItem('ArrayToDos')) || []
        if (editIndex.value !== null) {
            TodoData[editIndex.value] = { ...dataTodo.value };
            editIndex.value = null;
        } else {
            TodoData.push({ ...dataTodo.value });
        }
        localStorage.setItem('ArrayToDos', JSON.stringify(TodoData))
        savedDataArray.value = TodoData
        dataTodo.value = { title: '', ToDo: '' , date: '', time: '', category: ''}
    }

    const editItem = (index) => {
        dataTodo.value = { ...savedDataArray.value[index] };
        editIndex.value = index;
    };


    const deleteItem = (index) => {
            const TodoData = JSON.parse(localStorage.getItem('ArrayToDos')) || []
            TodoData.splice(index, 1)
            localStorage.setItem('ArrayToDos', JSON.stringify(TodoData))
            savedDataArray.value = TodoData
    }

    if (localStorage.getItem('ArrayToDos')) {
        savedDataArray.value = JSON.parse(localStorage.getItem('ArrayToDos'))
    }

    return {
        dataTodo,
        addDataToLocalStorage,
        savedDataArray,
        editItem,
        deleteItem
    }
  },
}
</script>

<template>
    <div class="max-w-md mx-auto flex flex-col justify-center ">
        <form @submit.prevent="addDataToLocalStorage">
            <div class="mb-6">
                <label for="title" class="block mb-2 text-sm font-medium text-gray-900">Title</label>
                <input type="text" id="title" v-model="dataTodo.title" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"/>
            </div>
            <div class="mb-6">
                <label for="ToDo" class="block mb-2 text-sm font-medium text-gray-900">To Do</label>
                <textarea type="text" id="ToDo" v-model="dataTodo.ToDo" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"></textarea>
            </div>
            <div class="grid gap-x-6 mb-6 grid-cols-2">
                <p>Date (optional)</p>
                <p>Time (optional)</p>
                <div>
                    <input type="date" id="first_name" v-model="dataTodo.date" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" placeholder="John" />
                </div>
                <div>
                    <input type="time" v-model="dataTodo.time" class="bg-gray-50 border border-gray-500 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 "/>
                </div>
            </div>
            <div>
                <p>Priority</p>
                <div class="grid grid-cols-3 gap-6">
                    <input type="radio" id="important" v-model="dataTodo.priority" name="priority." value="important" class="hidden peer/important justify-center">
                    <label for="important" class="peer-checked/important:text-white peer-checked/important:bg-red-500 border rounded-lg border-gray-500 focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">important</label>
                    <input type="radio" id="urgent" v-model="dataTodo.priority" name="priority." value="urgent" class="hidden peer/urgent">
                    <label for="urgent" class="peer-checked/urgent:text-white peer-checked/urgent:bg-yellow-500 border rounded-lg border-gray-500 focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">urgent</label>
                    <input type="radio" id="deferred" v-model="dataTodo.priority" name="priority." value="deferred" class="hidden peer/deferred">
                    <label for="deferred" class="peer-checked/deferred:text-white peer-checked/deferred:bg-green-500 border rounded-lg border-gray-500 focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">deferred</label>
                </div>
            </div>
            <div class="grid grid-cols-3 gap-6">
                <input type="radio" v-model="dataTodo.category" name="category" checked>
                <input type="radio" v-model="dataTodo.category" name="category">
                <input type="radio" v-model="dataTodo.category" name="category">
            </div>
            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-500 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center">
                Save
            </button>
        </form>
        <ul>
            <li v-for="(data, index) in savedDataArray" :key="index">
                Judul: {{ data.title }}, ToDo: {{ data.ToDo }}, Date: {{  data.date || '-' }}, time: {{ data.time || '-' }}, priority: {{ data.priority }}
                <button @click="editItem(index)" class="border text-yellow-500">edit</button>
                <button @click="deleteItem(index)" class="border text-red-500">delete</button>
            </li>
        </ul>
    </div>
</template>
