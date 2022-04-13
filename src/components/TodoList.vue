<template>
    <ul class="todo-main">
      <transition-group name="todo" appear>
          <TodoItem
              v-for="todoObj in todos"
              :key="todoObj.id"
              :todo="todoObj"
          />
      </transition-group>
    </ul>
</template>

<script>
    import TodoItem from './TodoItem'
    export default {
        name:'TodoList',
        components:{TodoItem},
        props:['todos'],
    }
</script>

<style scoped>
    /*main*/
    .todo-main {
      margin-left: 0px;
      border: 1px solid #ddd;
      border-radius: 2px;
      padding: 0px;
    }

    .todo-enter-active {
        animation: trans 0.5s linear;
    }

    .todo-leave-active {
        animation: trans 0.5s linear reverse;
    }

    @keyframes trans {
        from {
            transform: translateX(100%);
        }
        to {
            transform: translateX(0);
        }
    }
</style>