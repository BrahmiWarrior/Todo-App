<template>
  <q-page-container class="q-pa-md" padding>
    <q-page>
      <div class="input-group" style="max-width: 300px">
        <q-input
          filled
          v-model="title"
          label="Title*"

          color="white"
        />
       <q-input
          filled
          v-model="description"
          label="Description * "

color="white"
        />
        <div>
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
        <div class="col-auto">
          <div class="text-h3 text-weight-bold text-center">
            CREATED:{{ createdCount }}
          </div>
          <todo
            v-for="todo in createdTodos"
            :key="todo.id"
            :todoitem="todo"
            @refreshData="refreshData"
          >
          </todo>
        </div>
        <div class="col-auto">
          <div class="text-h3 text-weight-bold text-center">
            INPROGRESS:{{ inprogressCount }}
          </div>
          <todo
            v-for="todo in inProgressTodos"
            :key="todo.id"
            :todoitem="todo"
            @refreshData="refreshData"
          >
          </todo>
        </div>
        <div class="col-auto">
          <div class="text-h3 text-weight-bold text-center">
            COMPLETED:{{ completedCount }}
          </div>
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
export default {
  components: { todo },
  name: "PageIndex",

  data() {
    return {
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
        createdOn: new Date().toLocaleDateString(),
        updatedOn: new Date().toLocaleDateString(),
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

    editTodo(index, index2, index3, index4, index5) {
      let updated = [...JSON.parse(localStorage.getItem("todos"))];
      updated.forEach((data) => {
        if (data.id === index) {
          data.description = index3;
          data.title = index2;
          data.status = index4;
          data.updatedOn = index5;
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
    inProgressTodos() {
      return this.todos.filter((todo) => todo.status == "inprogress");
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

<style>
.text-h3 {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-size: 3.75;
  font-weight: 300;
  line-height: 3.75rem;
  letter-spacing: -0.00833em;
}
.q-page-container {
  background: linear-gradient(270deg, #ade0ff, #193D3C);
  -webkit-animation: waves 10s ease infinite;
  -moz-animation: waves 10s ease infinite;
  background-size: 200%;
  animation: waves 10s ease infinite;

}
@-webkit-keyframes waves {
    0%{background-position:0% 50%}
    50%{background-position:100% 50%}
    100%{background-position:0% 50%}
}
@-moz-keyframes waves {
    0%{background-position:0% 50%}
    50%{background-position:100% 50%}
    100%{background-position:0% 50%}
}
@keyframes waves {
    0%{background-position:0% 50%}
    50%{background-position:100% 50%}
    100%{background-position:0% 50%}
}
.q-pa-xs {
  padding: 18px 0px;
}

.q-card {
  border-radius: 30px;
  transition: all 300ms;
}
.button1 {
  border: none;
  position: relative;
  width: 200px;
  height: 73px;
  padding: 0;
  z-index: 2;
  -webkit-mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='868' width='2500' viewBox='0 0 726 252.17'%3E%3Cpath d='M483.92 0S481.38 24.71 466 40.11c-11.74 11.74-24.09 12.66-40.26 15.07-9.42 1.41-29.7 3.77-34.81-.79-2.37-2.11-3-21-3.22-27.62-.21-6.92-1.36-16.52-2.82-18-.75 3.06-2.49 11.53-3.09 13.61S378.49 34.3 378 36a85.13 85.13 0 0 0-30.09 0c-.46-1.67-3.17-11.48-3.77-13.56s-2.34-10.55-3.09-13.61c-1.45 1.45-2.61 11.05-2.82 18-.21 6.67-.84 25.51-3.22 27.62-5.11 4.56-25.38 2.2-34.8.79-16.16-2.47-28.51-3.39-40.21-15.13C244.57 24.71 242 0 242 0H0s69.52 22.74 97.52 68.59c16.56 27.11 14.14 58.49 9.92 74.73C170 140 221.46 140 273 158.57c69.23 24.93 83.2 76.19 90 93.6 6.77-17.41 20.75-68.67 90-93.6 51.54-18.56 103-18.59 165.56-15.25-4.21-16.24-6.63-47.62 9.93-74.73C656.43 22.74 726 0 726 0z'/%3E%3C/svg%3E")
    no-repeat 50% 50%;
  mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='868' width='2500' viewBox='0 0 726 252.17'%3E%3Cpath d='M483.92 0S481.38 24.71 466 40.11c-11.74 11.74-24.09 12.66-40.26 15.07-9.42 1.41-29.7 3.77-34.81-.79-2.37-2.11-3-21-3.22-27.62-.21-6.92-1.36-16.52-2.82-18-.75 3.06-2.49 11.53-3.09 13.61S378.49 34.3 378 36a85.13 85.13 0 0 0-30.09 0c-.46-1.67-3.17-11.48-3.77-13.56s-2.34-10.55-3.09-13.61c-1.45 1.45-2.61 11.05-2.82 18-.21 6.67-.84 25.51-3.22 27.62-5.11 4.56-25.38 2.2-34.8.79-16.16-2.47-28.51-3.39-40.21-15.13C244.57 24.71 242 0 242 0H0s69.52 22.74 97.52 68.59c16.56 27.11 14.14 58.49 9.92 74.73C170 140 221.46 140 273 158.57c69.23 24.93 83.2 76.19 90 93.6 6.77-17.41 20.75-68.67 90-93.6 51.54-18.56 103-18.59 165.56-15.25-4.21-16.24-6.63-47.62 9.93-74.73C656.43 22.74 726 0 726 0z'/%3E%3C/svg%3E")
    no-repeat 50% 50%;
  -webkit-mask-size: 100%;
  cursor: pointer;
  background-color: transparent;
  transform: translateY(8px);
}

.button1:after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;

  transition: all 2s ease;
}
.button1:before {
  content: "";
  position: absolute;
  width: 0;
  height: 100%;
  background-color: rgb(0, 0, 0);
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  transition: all 1s ease;

}
.button1 span {
 position: absolute;
 font-size: 15px;
 font-weight: 100;
 left: 50%;
 top: 45%;
 letter-spacing: 3px;
 text-align: center;
font-weight: bold;
 transform: translate(-50%,-50%);
 color: black;
 transition: all 2s ease;
}
.button1:hover span {
 color: white;
}
.button1:hover:after {
 box-shadow: 0px -13px 56px 12px #ffffffa6;
}
.button1:hover:before {
  width: 100%;
  color: white;
}


.input-group{
color: white;
}




</style>
