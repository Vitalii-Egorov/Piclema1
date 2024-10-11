<template>
    <div class="container">
      <h1>Список дел</h1>
      <input
        type="text"
        v-model="newTask"
        placeholder="Добавить задачу"
        @keyup.enter="addTask"
      />
      <button @click="addTask">Добавить</button>
      <ul>
        <li
          v-for="(task, index) in tasks"
          :key="index"
          :class="{ completed: task.completed }" 
        >
        <div><input
            type="checkbox"
            v-model="task.completed"
            @change="toggleTask(index)"
          />
          <span>{{ task.text }}</span></div>
          
         <div> <button @click="editTask(index)">Редактировать</button>
          <button @click="deleteTask(index)">Удалить</button></div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { ref, computed } from 'vue';
  
  export default {
    setup() {
      // Вычисляемые свойства
      const completedTasksCount = computed(() => {
        return tasks.value.filter((task) => task.completed).length;
      });
  
      // Функции
      const addTask = () => {
        if (newTask.value.trim() !== '') {
          tasks.value.push({
            text: newTask.value,
            completed: false,
          });
          newTask.value = '';
          saveTasksToLocalStorage();
        }
      };
  
      const toggleTask = (index) => {
        tasks.value[index].completed = !tasks.value[index].completed;
        tasks.value[index].completed = !tasks.value[index].completed;
        saveTasksToLocalStorage();
      };
  
      const editTask = (index) => {
        const editedTask = prompt('Редактировать задачу:', tasks.value[index].text);
        if (editedTask !== null) {
          tasks.value[index].text = editedTask;
          saveTasksToLocalStorage();
        }
      };
  
      const deleteTask = (index) => {
        tasks.value.splice(index, 1);
        saveTasksToLocalStorage();
      };
  
      const loadTasksFromLocalStorage = () => {
        const storedTasks = localStorage.getItem('tasks');
        return storedTasks ? JSON.parse(storedTasks) : [];
      };
  
      const saveTasksToLocalStorage = () => {
        localStorage.setItem('tasks', JSON.stringify(tasks.value));
      };
     // Состояние задач
      const tasks = ref(loadTasksFromLocalStorage());
      const newTask = ref('');
      return {
        tasks,
        newTask,
        addTask,
        toggleTask,
        editTask,
        deleteTask,
        completedTasksCount,
      };
    },
  };
  </script>
  
  <style>
  .container {
    margin: 20px;
  }
   ul{
    display: flex;
    flex-direction: column-reverse
   }
  li {
    display: flex;
    justify-content: space-between;
    list-style: none;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  .completed {
    text-decoration: line-through;
    color: red; 
  }
  
  </style>
  