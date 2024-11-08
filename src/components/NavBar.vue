<script setup>

</script>

<template>
    <nav class="bg-white border-gray-200 dark:bg-gray-900">
        <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
            <div class="block w-auto" id="navbar-default">
                <ul class="font-medium flex border-gray-100 rounded-lg flex-row space-x-8 rtl:space-x-reverse mt-0 border-0 bg-white dark:bg-gray-900 dark:border-gray-700">
                    <li>
                        <RouterLink to="/" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 hover:bg-transparent border-0 hover:text-blue-700 p-0 dark:text-white dark:hover:bg-gray-700 dark:hover:text-white dark:hover:bg-transparent">Home</RouterLink>
                    </li>
                    <li>
                        <RouterLink to="/notes" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 hover:bg-transparent border-0 hover:text-blue-700 p-0 dark:text-white dark:hover:bg-gray-700 dark:hover:text-white dark:hover:bg-transparent">Notes</RouterLink>
                    </li>
                    <li>
                        <RouterLink to="/todo" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 hover:bg-transparent border-0 hover:text-blue-700 p-0 dark:text-white dark:hover:bg-gray-700 dark:hover:text-white dark:hover:bg-transparent">ToDo</RouterLink>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

</template>