<template>
<div class="home container">
  <h1>Todo</h1>
  <input type="text" class="todo-input" placeholder="What needs to be done ?" v-model="newTodo"  @keyup.enter="addTodo">
  <p>Press 'enter' key to add TODO</p>

   <div>
    <a class="todo-btn" @click="toggle">View List</a>
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item" v-show="isShown">
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{completed: todo.completed}">{{ todo.title }} </div>
          <input v-else type="text" v-model="todo.title" class="todo-item-edit" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"  @keyup.esc="cancelEdit(todo)" >
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
      </div>
    </transition-group>
    <div>
      <transition name="fade">
        <button v-if="showClearCompletedButton" @click="clearCompleted" class="clear-btn">Clear completed</button>
      </transition>
    </div>
   </div>

 
</div>
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      newTodo: '',
      idForTodo: 0,
      beforeEditCache: '',
      isShown: false,
      todos: [
        {
          'id': 1,
          'title': 'Learn vue.js',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': 'Learn node.js',
          'completed': false,
          'editing': false,
        },
        {
          'id': 3,
          'title': 'Read some books',
          'completed': true,
          'editing': false,
        }
      ]
    }
  },

  computed: {
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },

  methods: {
    addTodo() {
      if(this.newTodo.trim().length == 0) {
        return;
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })

      this.newTodo = '';
      this.idForTodo++;
    },
     
    editTodo(todo){
      this.beforeEditCache = todo.title
      todo.editing = true
    },

    doneEdit(todo) {
      todo.editing = false
      if(todo.title.trim().length == 0) {
        todo.title = this.beforeEditCache
      }
    },

    cancelEdit(todo){
      todo.title = this.beforeEditCache
      todo.editing = false
    },

    removeTodo(index) {
      this.todos.splice(index, 1)
    },

  clearCompleted() {
    this.todos = this.todos.filter(todo => !todo.completed)
  },
  
  toggle() {
      // Toggles control property
      this.isShown = !this.isShown;
    }, 
  todoCompleted() {
    //completed todos
    todos = this.todos.filter(todo => todo.completed)
  } 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.css);
 
.container{
  max-width: 600px;
  margin: 0 auto;
}

.todo-input {
  min-width: 600px;
  padding: 10px 18px;
  font-size: 18px;
  margin: 16px 0;
  outline: none;
}

.todo-item {
  margin-bottom: 6px;
  margin-top: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border: 1px solid currentColor;
  padding: 5px 10px;
  animation-duration: .9s;
}

.todo-item-left {
    display: flex;
    align-items: center;
  }

.remove-item {
  cursor: pointer;
  margin-left: 14px;
}

.remove-item:hover {
      color: red;
      transform: scale(1.5);
      transition: .1s;
    }

.completed {
  text-decoration: line-through;
  color: grey;
}

.todo-btn {
  font-size: 14px;
  background-color: #fff;
  border: 2px solid #ccc;
  padding: 5px 10px;
  color: currentColor;
  outline: none;
  cursor: pointer;
}

.clear-btn {
  margin-top: 15px;
}

.fade-enter-active, .fade-leave-active {
transition: opacity .2s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
