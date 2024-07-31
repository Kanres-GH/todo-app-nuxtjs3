<template>
  <div class="h-screen w-screen flex justify-center items-center bg-gray-100">
    <div class="container mx-auto p-4">
      <div class="todo-content flex flex-col items-center">
        <h1 class="text-center mt-3 text-3xl md:text-5xl rounded-full"><i>To-Do List Manager</i></h1>
        <h2 class="text-center mt-5 mb-3 text-lg md:text-2xl">Enter a task name</h2>
        <div class="todo-form flex flex-col items-center gap-5">
          <input
            type="text"
            id="inputt"
            v-model="inputValue"
            @input="clearWarningMessage"
            class="w-full md:w-1/2 px-4 py-2 rounded-lg text-lg md:px-4 md:py-4"
            placeholder="E.g. 'Make a sandwich'"
            required
          />
          <button class="btn w-full md:w-1/2" :disabled="!isInputValid" @click="addTask">Add the task</button>
          <p v-if="warningMessage" class="text-red-500 text-center">{{ warningMessage }}</p>
        </div>
        <ul class="mt-5 w-full md:w-1/2">
          <li
            v-for="(task, index) in tasks"
            :key="index"
            class="task-item flex justify-between items-center p-2 mb-2 bg-white rounded-lg shadow-md"
          >
            <div v-if="task.isEditing" class="flex-grow">
              <input
                type="text"
                v-model="task.newName"
                class="w-full px-2 py-1 rounded-lg"
                placeholder="Edit task name"
              />
            </div>
            <div v-else class="flex-grow">
              <span>{{ task.name }}</span>
            </div>
            <div class="flex-shrink-0 ml-2">
              <button
                v-if="task.isEditing"
                @click="saveTask(index)"
                class="btn text-xs"
              >
                Save
              </button>
              <button
                v-else
                @click="editTask(index)"
                class="btn text-xs"
              >
                Edit
              </button>
              <button
                @click="deleteTask(index)"
                class="btn btn-red text-xs"
              >
                Delete
              </button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const inputValue = ref('');
const tasks = ref([]);
const warningMessage = ref('');

onMounted(() => {
  loadTasksFromLocalStorage();
});

const isInputValid = computed(() => inputValue.value.trim() !== '');

const clearWarningMessage = () => {
  warningMessage.value = '';
};

const addTask = () => {
  if (!isInputValid.value) {
    warningMessage.value = 'Please enter a task name';
    return;
  }

  tasks.value.push({
    name: inputValue.value.trim(),
    isEditing: false,
    newName: '',
  });

  inputValue.value = '';
  saveTasksToLocalStorage();
};

const editTask = (index) => {
  tasks.value[index].isEditing = true;
  tasks.value[index].newName = tasks.value[index].name;
};

const saveTask = (index) => {
  if (tasks.value[index].newName.trim() === '') {
    warningMessage.value = 'Task name cannot be empty';
    return;
  }

  tasks.value[index].name = tasks.value[index].newName.trim();
  tasks.value[index].isEditing = false;
  tasks.value[index].newName = '';
  saveTasksToLocalStorage();
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  saveTasksToLocalStorage();
};

const saveTasksToLocalStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

const loadTasksFromLocalStorage = () => {
  const storedTasks = JSON.parse(localStorage.getItem('tasks')) || [];
  tasks.value = storedTasks;
};
</script>

<style scoped>

</style>
