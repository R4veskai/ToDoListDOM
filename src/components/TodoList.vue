<template>
  <div class="todo-container">
    <h1>Список важных дел</h1>
    
    <div class="input-container">
        
            <input 
              v-model="newTask" 
              @keyup.enter="addTask" 
              placeholder="Че надо сделать..."
              class="task-input"
            >
       
      <button @click="addTask" class="add-btn">Добавить</button>
    </div>
    
    <ul class="task-list">
      <li 
        v-for="(task, index) in tasks" 
        :key="index" 
        :class="{ completed: task.completed }"
        @click="toggleCompleted(index)"
      >
        {{ task.text }}
        <div @click.stop="removeTask(index)" class="delete-btn">
            <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="20px" height="20px" viewBox="0 0 24 24">
  <defs>
    <path id="cross-a" d="M0.292893219,1.70710678 C-0.0976310729,1.31658249 -0.0976310729,0.683417511 0.292893219,0.292893219 C0.683417511,-0.0976310729 1.31658249,-0.0976310729 1.70710678,0.292893219 L9.70710678,8.29289322 C10.0976311,8.68341751 10.0976311,9.31658249 9.70710678,9.70710678 C9.31658249,10.0976311 8.68341751,10.0976311 8.29289322,9.70710678 L0.292893219,1.70710678 Z"/>
    <path id="cross-c" d="M3.58578644,5 L0.292893219,1.70710678 C-0.0976310729,1.31658249 -0.0976310729,0.683417511 0.292893219,0.292893219 C0.683417511,-0.0976310729 1.31658249,-0.0976310729 1.70710678,0.292893219 L5,3.58578644 L8.29289322,0.292893219 C8.68341751,-0.0976310729 9.31658249,-0.0976310729 9.70710678,0.292893219 C10.0976311,0.683417511 10.0976311,1.31658249 9.70710678,1.70710678 L6.41421356,5 L9.70710678,8.29289322 C10.0976311,8.68341751 10.0976311,9.31658249 9.70710678,9.70710678 C9.31658249,10.0976311 8.68341751,10.0976311 8.29289322,9.70710678 L5,6.41421356 L1.70710678,9.70710678 C1.31658249,10.0976311 0.683417511,10.0976311 0.292893219,9.70710678 C-0.0976310729,9.31658249 -0.0976310729,8.68341751 0.292893219,8.29289322 L3.58578644,5 Z"/>
  </defs>
  <g fill="none" fill-rule="evenodd">
    <g transform="translate(8 6)">
      <mask id="cross-b" fill="#ffffff">
        <use xlink:href="#cross-a"/>
      </mask>
      <use fill="#D8D8D8" fill-rule="nonzero" xlink:href="#cross-a"/>
      <g fill="#FFA0A0" mask="url(#cross-b)">
        <rect width="24" height="24" transform="translate(-8 -6)"/>
      </g>
    </g>
    <g transform="rotate(-90 12 5)">
      <mask id="cross-d" fill="#ffffff">
        <use xlink:href="#cross-c"/>
      </mask>
      <use fill="#000000" fill-rule="nonzero" xlink:href="#cross-c"/>
      <g fill="#7600FF" mask="url(#cross-d)">
        <rect width="24" height="24" transform="translate(-7 -7)"/>
      </g>
    </g>
  </g>
</svg>
        </div>
      </li>
    </ul>
    
    <div v-if="tasks.length === 0" class="empty-message">
      Нет задач. Добавь её!
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data() {
    return {
      newTask: '',
      tasks: []
    }
  },
  mounted() {
    this.loadTasks();
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === '') return;
      
      this.tasks.push({
        text: this.newTask.trim(),
        completed: false
      });
      
      this.newTask = '';
      this.saveTasks();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    toggleCompleted(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('vue-todo-tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const savedTasks = localStorage.getItem('vue-todo-tasks');
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks);
      }
    }
  }
}
</script>

<style scoped>
.todo-container {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: 100%;
  max-width: 500px;
}

h1 {
  color: #333;
  text-align: center;
  margin-bottom: 20px;
}

.input-container {
  max-width: 450px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.task-input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
  font-size: 16px;
  outline: none;
  margin-bottom: 14px;
}

.task-input:focus {
  border-color: #ff4f4f;
}

.add-btn {
  padding: 10px 15px;
  background-color: #fc4545;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
  max-width: 150px;
}

.add-btn:hover {
  background-color: #c50505;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  color: black;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 15px;
  border-bottom: 1px solid #eee;
  cursor: pointer;
  transition: background-color 0.2s;
}

.task-list li:hover {
  background-color: #f9f9f9;
}

.task-list li.completed {
  text-decoration: line-through;
  color: #0f0f0f;
}


.delete-btn {
  display: flex;
  cursor: pointer;
  padding: 0 5px;
  
  text-decoration: none;
}

.empty-message {
  text-align: center;
  color: #888;
  padding: 20px;
}
</style>