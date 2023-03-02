<template>
  <q-page-container class="q-pa-md" padding>
    <q-page>
      <div class="input-group" style="max-width: 400px">
        <q-input
          class="q-pa-md"
          :rules="[(val) => !!val || 'Field is required']"
          v-model="title"
          label="Title"
          color="black"
          filled
        />
        <q-input
          class="q-pa-md"
          :rules="[(val) => !!val || 'Field is required']"
          v-model="description"
          label="Description"
          color="black "
          filled
        />

        <div class="q-pa-md">
          <q-btn
            class="button1"
            label="Submit"
            color="white"
            @click="addTodo"
            :disabled="isDisable"
          />
        </div>
      </div>

      <hr />
      <br />
      <div class="fit row justify-evenly">
        <div class="col-auto q-py-xl cardwidth">
          <q-card class="text-h4 border1" >
            <span> CREATED:{{ createdCount }}</span
            ><i></i>
          </q-card>

            <todo
              v-for="todo in createdTodos"
              :key="todo.id"
              :todoitem="todo"
              @refreshData="refreshData"
            >
            </todo>

        </div>
        <div class="col-auto q-py-xl cardwidth">
          <q-card class="text-h4 border2">
            <span>INPROGRESS:{{ inprogressCount }}</span
            ><i></i>
          </q-card>
            <todo
              v-for="todo in inProgressTodos"
              :key="todo.id"
              :todoitem="todo"
              @refreshData="refreshData"
            >
            </todo>
        </div>
        <div class="col-auto q-py-xl cardwidth">
          <q-card class="text-h4 border3">
            <span> COMPLETED:{{ completedCount }}</span
            ><i></i>
          </q-card>

            <todo
              v-for="todo in completedTodos"
              :key="todo.id"
              :todoitem="todo"
              @refreshData="refreshData"
            >
            </todo>

        </div>
      </div>
    </q-page>
  </q-page-container>
</template>

<script>
import { uid } from "quasar";

import todo from "../components/Todo.vue";
import frontpage from "../css/frontpage.css";
export default {
  components: { todo },
  name: "PageIndex",

  data() {
    return {
      dummylist: [],
      todos: [],
      title: "",
      description: "",
      selected: [],
      confirm: false,
      options: ["created", "inprogress", "completed"],
    };
  },
  methods: {

    addTodo() {
      var todo = {
        id: uid(),
        title: this.title,
        description: this.description,
        status: "created",
        createdOn: new Date().toLocaleString(),
        updatedOn: new Date().toLocaleString(),
      };
      //local storage get and set
      var todos = localStorage.getItem("todos") || "[]";
      todos = JSON.parse(todos);
      todos.push(todo);
      this.todos = todos;
      todos = JSON.stringify(todos);
      localStorage.setItem("todos", todos);
      this.title = "";
      this.description = "";
    },

    editTodo(id, title, description, status, updatedOn) {
      let updated = [...JSON.parse(localStorage.getItem("todos"))];
      updated.forEach((data) => {
        if (data.id === id) {
          data.description = description;
          data.title = title;
          data.status = status;
          data.updatedOn = updatedOn;
        }
      });
      localStorage.setItem("todos", JSON.stringify(updated));
    },

    removeTodo() {
      const selectedTodo = this.selected[0];
      const index = this.todos.findIndex((todo) => todo.id == selectedTodo.id);
      this.todos.splice(index, 1);
      const todos = JSON.stringify(this.todos);
      localStorage.setItem("todos", todos);
      this.selected = [];
    },
    refreshData() {
      var todos = localStorage.getItem("todos") || "[]";
      todos = JSON.parse(todos);
      this.todos = todos;
    },
  },
  mounted() {
    this.refreshData();
  },
  computed: {
    createdCount() {
      return this.todos.filter((el) => el.status === "created").length;
    },
    inprogressCount() {
      return this.todos.filter((el) => el.status === "inprogress").length;
    },
    completedCount() {
      return this.todos.filter((el) => el.status === "completed").length;
    },
    todoInfo() {
      if (this.selected.length != 0) {
        return this.selected[0];
      }
      return { title: "", description: "" };
    },
    createdTodos() {
      return this.todos.filter((todo) => todo.status == "created");
    },
    inProgressTodos: {
      get () {
        return this.todos.filter((todo) => todo.status == "inprogress");
      },
      set(newValue) {
        alert(newValue)
      }
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.status == "completed");
    },

    isDisable() {
      return this.title.length && this.description.length == 0;
    },

  },
};
</script>

<style></style>
