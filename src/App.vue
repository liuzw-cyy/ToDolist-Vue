<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader :addTodo="addTodo"/>
        <TodoList :todos="todos"/>
        <TodoFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
      </div>
    </div>
  </div>
</template>

<script>
    import TodoHeader from './components/TodoHeader'
    import TodoFooter from './components/TodoFooter'
    import TodoList from './components/TodoList'


    export default {
      name: 'App',
      components:{TodoHeader, TodoFooter, TodoList},
      data() {
        return {
          // 从本地存储中获得数据，如果为null就创建空数组[]
            todos:JSON.parse(localStorage.getItem('todos')) || []
        }
      },
      methods: {
        //添加一个todo
        addTodo(todoObj){
          this.todos.unshift(todoObj)
        },
        //勾选or取消勾选一个todo
        checkTodo(id){
          this.todos.forEach((todo) => {
            if(todo.id === id) {
              todo.done = !todo.done
            }
          })
        },
        deleteTodo(id) {
          this.todos = this.todos.filter(todo => {
              return todo.id !== id
          })
        },
        //全选or取消全选
        checkAllTodo(done) {
          this.todos.forEach((todo) => {
              todo.done = done
          })
        },
        //清除所有已经完成的todo
        clearAllTodo(){
          this.todos = this.todos.filter((todo) => {
            return !todo.done
          })
        },
        //更新一个todo
        updateTodo(id, title) {
          this.todos.forEach((todo) => {
            if(todo.id === id) {
              todo.title = title
            }
          })
        }
      },
      // 将输入事件放到浏览器本地存储中
      watch:{
        todos:{
          // 开启深度监视
          deep:true,
          handler(value) {
            localStorage.setItem('todos', JSON.stringify(value))
          }
        }
      },
      // 全局事件总线
      mounted() {
        this.$bus.$on('checkTodo', this.checkTodo)
        this.$bus.$on('deleteTodo', this.deleteTodo)
        this.$bus.$on('updateTodo',this.updateTodo)
      },
      beforeDestroy() {
        this.$bus.$off('checkTodo')
        this.$bus.$off('deleteTodo')
        this.$bus.$off('updateTodo')
      }
    }
</script>

<style>
    /*base*/
    body {
      background: #fff;
    }

    .btn {
      display: inline-block;
      padding: 4px 12px;
      margin-bottom: 0;
      font-size: 14px;
      line-height: 20px;
      text-align: center;
      vertical-align: middle;
      cursor: pointer;
      box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
      border-radius: 4px;
    }

    .btn-danger {
      color: #fff;
      background-color: #da4f49;
      border: 1px solid #bd362f;
    }

    .btn-edit {
      background-color: bisque;
      margin-right: 5px;
    }

    .btn-danger:hover {
      color: #fff;
      background-color: #bd362f;
    }

    .btn:focus {
      outline: none;
    }

    .todo-container {
      width: 600px;
      margin: 0 auto;
    }
    .todo-container .todo-wrap {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }


</style>
