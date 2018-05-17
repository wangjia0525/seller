<template>
  <div id="todo-list-example">
    <input
      class="search"
      v-model="newTodoText"
      v-on:keyup.enter="addNewTodo"
      placeholder="Add a todo"
    >
    <ul>
      <li
        is="todo-item"
        v-for="(todo, index) in todos"
        v-bind:key="todo.id"
        v-bind:title="todo.title"
        v-on:remove="todos.splice(index, 1)"
      ></li>
    </ul>
  </div>
</template>
<script>
  import Vue from 'vue';
  Vue.component('todo-item', {
  template: `<li>
              {{ title }}
              <button v-on:click="$emit('remove')">X</button>
            </li>`,
  props: ['title']
})
  export default {
      data(){
        return {
          newTodoText: '',
          todos: [
            {
              id: 1,
              title: 'Do the dishes',
            },
            {
              id: 2,
              title: 'Take out the trash',
            },
            {
              id: 3,
              title: 'Mow the lawn'
            }
          ],
          nextTodoId: 4
        } 
      },
      methods: {
        addNewTodo: function () {
          this.todos.push({
            id: this.nextTodoId++,
            title: this.newTodoText
          })
          this.newTodoText = ''
        }
      }
  };


</script>

<style>
  .search{
    height:20px;
    margin:0 5%;
    padding:5px;
    border:1px solid #eee;
    font-size: 16px;
  }
</style>
