<template>
<section class="bg-white p-5">
  <form @submit.prevent="addTodo()">
    
    <input class="bg-transparent border border-transparent w-96 mr-3 focus:outline-none focus:ring-0 focus:border-transparent"
      v-model="newTodo"
      name="newTodo"
      autocomplete="off"
      placeholder="What needs to be done?"
    >
  </form>
  <div class="flex">
    <ul>
      <li 
      v-for="(todo, index) in todos"
     :key="index">
       <div class="flex justify-center items-center">
         <label><input @click="doneTodo(todo)" class="mr-3 my-2 text-emerald-500 focus:ring-0 focus:ring-offset-0 rounded-full border-slate-200 h-6 w-6" type="checkbox" v-model="todo.done" />
          </label>
          <p class="items w-96" @click="doneTodo(todo)">{{ todo.content }}</p>
          <button @click="removeTodo(index)">x</button>
       </div>

      </li>
    </ul>
  </div>
  <h4 h4 v-if="todos.length === 0">Empty list.</h4>
  <div class="flex " >
    <p>{{todos.length}}items</p>
    <div class="px-10">
      <a href="/" class="px-3" >All</a>
      <a  class="px-3" @click="activeitem(todos)">Active</a>
      <a  class="px-3"  @click="completed(todos)">Completed</a>
    </div>
     <div>
       <a href="/clearCompleted" @click="clearTodo(todos)">Clear completed</a>
    </div>
</div>
</section>
</template>

<script>
import { ref } from 'vue';
export default {
name: 'TodoApp',
setup () {
    let idItem = 0;
    const newTodo = ref('');
    const todosData = JSON.parse(localStorage.getItem('todos')) || [];
    const todos = ref(todosData);
    function addTodo () {
      if (newTodo.value) {
      todos.value.push({
        id: idItem,
        done: false,
        content: newTodo.value
      });
      newTodo.value = '';
      idItem++;
      }
      saveData();
    }

    function doneTodo (todo) {
      todo.done = !todo.done
      saveData();
    }

    function removeTodo (index) {
      todos.value.splice(index, 1);
      saveData();
    }

    function clearTodo(todos){
      console.log(todos[0].done)
      todos =todos.filter(data => data.done != true);
      const storageData = JSON.stringify(todos);
      localStorage.setItem('todos', storageData);
    }

    function activeitem(todos){
      let datatodo = JSON.parse(localStorage.getItem('todos')) || [];
      this.todos =datatodo.filter(data => data.done != true);
    }


    function completed(todos){
      let datatodo = JSON.parse(localStorage.getItem('todos')) || [];
      this.todos =datatodo.filter(data => data.done != false);
    }

    function saveData () {
      console.log(todos)
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem('todos', storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      clearTodo,
      activeitem,
      completed,
      saveData
    }
  }
}
</script>