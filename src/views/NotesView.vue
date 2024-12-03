<script setup>
import { ref } from 'vue'

const formData = ref({
    title: '',
    note: ''
});

const savedDataArray = ref([]);

const addDataToLocalStorage = () => {
    const currentData = JSON.parse(localStorage.getItem('ArrayNotes')) || [];
    currentData.push({ ...formData.value });
    localStorage.setItem('ArrayNotes', JSON.stringify(currentData));
    savedDataArray.value = currentData;
    formData.value = { title: '', note: '' };
};

if (localStorage.getItem('ArrayNotes')) {
    savedDataArray.value = JSON.parse(localStorage.getItem('ArrayNotes'));
}
</script>

<template>
    <div class="max-w-md">      
        <form @submit.prevent="addDataToLocalStorage">
            <div class="mb-6">
                <label for="title" class="block mb-2 text-sm font-medium text-gray-900">Title</label>
                <input type="text" id="title" v-model="formData.title" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" required/>
            </div> 
            <div class="mb-6">
                <label for="Note" class="block mb-2 text-sm font-medium text-gray-900">Note</label>
                <input type="Note" id="Note" v-model="formData.note" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" required/>
            </div> 
            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center">Submit</button>
        </form>
        <ul>
            <li v-for="(data, index) in savedDataArray" :key="index">
                Judul: {{ data.title }}, Note: {{ data.note }}
            </li>
        </ul>
    </div>
</template>