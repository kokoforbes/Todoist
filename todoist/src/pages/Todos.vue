<template>
  <div class="todos">
    <h1>{{msg}}</h1>
    <Ol>
      <li class="done-item" :v-bind="todoCompleted" is="todo.title"></li>
    </Ol>
  </div>
</template>

<script>

export default {
  name: 'Todos',
    data(){
    return {
      msg: 'Todo List',
    }
  },
  mounted(){
    this.$root.$on('todos = this.todos.filter(todo => todo.completed)')
  }
}
</script>

<style scoped>
.done-item{
  margin: 12px auto 6px auto;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border: 1px solid currentColor;
  padding: 5px 10px;
  animation-duration: .9s;
  max-width: 600px;
}
</style>
