<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo" />
        <MyList :todos="todos" />
        <MyFooter
          :todos="todos"
          @checkAllTodo="checkAllTodo"
          @clearAll="clearAll"
        />
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import MyHeader from "./components/MyHeader.vue";
import MyList from "./components/MyList.vue";
import MyFooter from "./components/MyFooter.vue";
export default {
  name: "App",
  components: { MyHeader, MyFooter, MyList },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  methods: {
    addTodo(todoObj) {
      this.todos.unshift(todoObj);
    },
    changDone(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done;
      });
    },
    ListDelete(_, id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    updateTodo(id, title) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.title = title;
      });
    },
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.done = done;
      });
    },
    clearAll() {
      this.todos = this.todos.filter((todo) => {
        return !todo.done;
      });
    },
  },
  mounted() {
    this.$bus.$on("changDnoe", this.changDone);
    //this.$bus.$on("ListDelete", this.ListDelete);
    this.deletePubId = pubsub.subscribe("deleteTodo", this.ListDelete);
    this.$bus.$on("updateTdo", this.updateTodo);
  },
  /* 解绑 */
  beforeDestroy() {
    this.$bus.off("changDnoe");
    //this.$bus.off("ListDelete")
    pubsub.unsubscribe(this.deletePubId);
    this.$bus.off("updateTdo");
  },
};
</script>

<style>
/* * {
  margin: 0;
  padding: 0;
}
#app {
  margin: 15px auto;
  width: 600px;
  border: 1px solid;
} */
body {
  background-color: #fff;
}
.btn {
  /* display: inline-block; */
  padding: 4 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 5px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: rgb(253, 154, 6);
  border: 1px solid rgb(253, 146, 6);
}
.btn-edit:hover {
  color: #fff;
  background-color: rgb(253, 120, 6);
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
