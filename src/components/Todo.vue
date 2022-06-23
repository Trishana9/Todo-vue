<template>
<section class="bg-white shadow-xl">
  <div class="flex items-center bg-gray-100 p-5">
  <button @click="doneAll(todos)" v-on:dblclick="notDone(todos)"><i class="fa fa-angle-down text-2xl font-bold px-1" aria-hidden="true"></i></button>
  <form @submit.prevent="addTodo()">    
    <input class="bg-transparent border border-transparent w-96 mr-3 focus:outline-none focus:ring-0 focus:border-transparent"
      v-model="newTodo"
      name="newTodo"
      autocomplete="off"
      placeholder="What needs to be done?"
    >
  </form>
  </div>
  <div class="flex px-5">
    <ul>
      <li 
      v-for="(todo, index) in todos"
     :key="index">
       <div class="flex justify-center items-center">
         <label><input @click="doneTodo(todo)" class="mr-3 my-2 text-emerald-500 focus:ring-0 focus:ring-offset-0 rounded-full border-slate-200 h-6 w-6" type="checkbox" v-model="todo.done" />
          </label>
          <p class="items w-[410px] text-xl" @click="doneTodo(todo)">{{ todo.content }}</p>
          <button @click="removeTodo(index)"><i class="fa fa-trash text-xl hover:text-red-500" aria-hidden="true"></i></button>
       </div>

      </li>
    </ul>
  </div>
  <h4 h4 v-if="todos.length === 0">Empty list.</h4>
  <div class="flex px-5 py-2 border-2 border-transparent border-t-gray-200" >
    <p>{{todos.length}} items</p>
    <div class="px-10 ">
      <a href="/" class="px-3  hover:text-green-400" >All</a>
      <a  class="px-3 cursor-pointer hover:text-green-400" @click="activeitem(todos)">Active</a>
      <a  class="px-3 cursor-pointer hover:text-green-400"  @click="completed(todos)">Completed</a>
    </div>
     <div>
       <a href="/" class="hover:text-green-400" @click="clearTodo(todos)">Clear completed</a>
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

    function doneAll(todos){
      console.log(todos.length)
      for(let i = 0 ; i< todos.length; i++){
          todos[i].done = true;
      }
      const storageData = JSON.stringify(todos);
      localStorage.setItem('todos', storageData);
    }

    function notDone(todos){
      console.log('hello')
      for(let i = 0 ; i< todos.length; i++){
          todos[i].done = false;
      }
      const storageData = JSON.stringify(todos);
      localStorage.setItem('todos', storageData);
    }

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
      this.todos = JSON.parse(localStorage.getItem('todos')) || [];
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
      doneAll,
      notDone,
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