<template>
  <div class="h-screen w-screen content-center">
    <div class="second-bg">
      <div class="todo-content flex flex-col justify-self-center">
        <h1 class="text-center mt-3 rounded-full"><i>To-Do List Manager</i></h1>
        <h2 class="text-center mt-5 mb-3">Enter a task name</h2>
        <div class="todo-form flex justify-center flex-col gap-5">
          <input
            type="text"
            id="inputt"
            v-model="inputValue"
            @input="clearWarningMessage"
            class="w-full px-4 py-2 rounded-lg text-lg md:px-4 md:py-4"
            placeholder="E.g. 'Make a sandwich'"
            required
          />
          <button class="btn" :disabled="!isInputValid" @click="addTask">Add the task</button>
          <p v-if="warningMessage" class="text-red-500 text-center">{{ warningMessage }}</p>
        </div>
        <div class="task-list mt-5">
          <ul>
            <li v-for="(task, index) in tasks" :key="index" class="flex justify-between items-center bg-white p-4 mb-2 rounded-lg shadow">
              <div v-if="editingIndex === index">
                <input
                  type="text"
                  v-model="editingTaskName"
                  class="px-4 py-2 rounded-lg text-lg md:px-4 md:py-4 edit-btn"
                />
              </div>
              <div v-else>
                <span :class="{ 'line-through': task.completed }">{{ task.text }}</span>
              </div>
              <div class="flex gap-2">
                <button class="btn" @click="editTask(index)" v-if="editingIndex !== index">Edit</button>
                <button class="btn bg-green-500" @click="saveTask(index)" v-if="editingIndex === index">Save</button>
                <button class="btn bg-red-500" @click="deleteTask(index)">Delete</button>
                <button class="btn bg-blue-500" @click="toggleCompleteTask(index)">
                  {{ task.completed ? 'Undo' : 'Complete' }}
                </button>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const inputValue = ref('');
const tasks = ref([]);
const warningMessage = ref('');
const editingIndex = ref(null);
const editingTaskName = ref('');

const isInputValid = computed(() => inputValue.value.trim() !== '');

const loadTasks = () => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
};

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

const clearWarningMessage = () => {
  warningMessage.value = '';
};

const addTask = () => {
  if (!isInputValid.value) {
    warningMessage.value = 'Task name cannot be empty';
    return;
  }

  tasks.value.push({ text: inputValue.value, completed: false });
  inputValue.value = '';
  saveTasks();
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  saveTasks();
};

const toggleCompleteTask = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
  saveTasks();
};

const editTask = (index) => {
  editingIndex.value = index;
  editingTaskName.value = tasks.value[index].text;
};

const saveTask = (index) => {
  if (editingTaskName.value.trim() === '') {
    warningMessage.value = 'Task name cannot be empty';
    return;
  }

  tasks.value[index].text = editingTaskName.value;
  editingIndex.value = null;
  editingTaskName.value = '';
  saveTasks();
};

onMounted(() => {
  loadTasks();
});
</script>

<style scoped>
h3 {
  font-size: 2vw;
  font-weight: bold;
}

p {
  font-size: 1.6vw;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.line-through {
  text-decoration: line-through;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-list {
  margin-top: 20px;
}

.bg-blue-500 {
  background-color: #3b82f6;
}

.bg-red-500 {
  background-color: #ef4444;
}

.bg-green-500 {
  background-color: #10b981;
}

.edit-btn {
  border: 1px solid gray;
  width: 90%;
}

</style>
