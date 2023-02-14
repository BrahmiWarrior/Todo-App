<template>
  <div class="q-pa-xs">
    <q-card :class="statusColor">

      <q-card-section>
        <q-card-section class="q-pt-xs">
          <div class="row">
            <div class="col">
              <div class="text-h5 q-mt-sm q-mb-xs text-weight-medium">
                Title
              </div>
            </div>
            <div class="col text-h6 q-mt-sm q-mb-xs">
              {{ todoitem.title }}
            </div>
          </div>

          <div class="row">
            <div class="col">
              <div class="text-h5 q-mt-sm q-mb-xs text-weight-medium">
                Description
              </div>
            </div>
            <div class="col text-h6 q-mt-sm q-mb-xs">
              {{ todoitem.description }}
            </div>
          </div>

          <div class="row">
            <div class="col">
              <div class="text-h5 q-mt-sm q-mb-xs text-weight-medium">
                Status
              </div>
            </div>
            <div class="col text-h6 q-mt-sm q-mb-xs">
              {{ todoitem.status }}
            </div>
          </div>

          <q-separator />

          <div class="q-ml-md q-pt-lg" style="text-align: center">
            <q-icon name="today" class="white q-ml-xs" style="font-size: 2em" />
          </div>
          <q-dialog v-model="deleteTodoDialog" persistent>
               <q-card square class="shadow-24 text-center">
          <q-card-section class="bg-red-7">
            <h2 class="text-h5 text-white ">Would You like to delete</h2>

          </q-card-section>

          <q-card-section >
   <q-list >
        <q-item >
          <q-item-section >
            <q-item-label class="text-center text-h5">Title:{{ todoitem.title }}</q-item-label>
          </q-item-section>
        </q-item>
</q-list>
 <q-list>
        <q-item >
          <q-item-section>
            <q-item-label class="text-center text-h5">Description:{{ todoitem.description }}</q-item-label>
          </q-item-section>
        </q-item>
</q-list>

              </q-card-section>

              <q-card-actions align="right">
                <q-btn class="q-pl-xl" flat label="Cancel" color="primary" v-close-popup />
                <q-btn
                  flat
                  label="Confirm delete"
                  color="red"
                  @click="removeTodo()"
                  v-close-popup
                />
              </q-card-actions>
            </q-card>
          </q-dialog>

          <q-dialog
            v-model="editTodoDialog"
            @before-show="beforeShow"
            persistent
          >
            <q-card  style="width: 350px">
              <q-card-section >
                <br />
                <q-input
                  filled
                  v-model="title"
                  :readonly="status == 'completed'"
                  label="Title*"
                />
                <br />
                <q-input filled v-model="description" label="description*" />
                <br />
                <q-select
                  filled
                  v-model="status"
                  :options="options"
                  label="Standard"
                />
              </q-card-section>
              <q-card-actions align="right">
                <q-btn flat label="Cancel" color="primary" v-close-popup />

                <q-btn
                  flat
                  label="Submit"
                  color="red"
                  v-close-popup
                  @click="editTodo"
                />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </q-card-section>
      </q-card-section>
      <q-card-actions align="between">
        <q-btn
          icon="edit"
          flat
          round
          color="white"
          @click="editTodoDialog = true"
        />
        <q-btn
          icon="delete"
          flat
          round
          color="red"
          @click="deleteTodoDialog = true"
        />
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
export default {
  name: "ComponentName",
  props: {
    todoitem: {
      type: Object,
    },
  },
  data() {
    return {
      status: "",
      deleteTodoDialog: false,
      editTodoDialog: false,
      options: ["created", "inprogress", "completed"],
      title: "",
      description: "",
    };
  },
  methods: {
    removeTodo() {
      const key = this.todoitem.id;
      let updated = [...JSON.parse(localStorage.getItem("todos"))];
      const index = updated.findIndex((updated) => updated.id == key);
      updated.splice(index, 1);
      const todos = JSON.stringify(updated);
      localStorage.setItem("todos", todos);
      this.$emit("refreshData");
    },
    beforeShow() {
      this.status = this.todoitem.status;
      this.title = this.todoitem.title;
      this.description = this.todoitem.description;
    },
    editTodo() {
      let todos = JSON.parse(localStorage.getItem("todos"));
      const index = todos.findIndex((todo) => todo.id == this.todoitem.id);
      todos[index].status = this.status;
      todos[index].title = this.title;
      todos[index].description = this.description;
      todos[index].updatedOn = new Date().toLocaleDateString();
      localStorage.setItem("todos", JSON.stringify(todos));
      this.$emit("refreshData");
    },
  },

  computed: {
    statusColor() {
      if (this.todoitem.status == "created") {
        return "bg-light-blue";
      } else if (this.todoitem.status == "inprogress") {
        return "bg-orange";
      }
      return "bg-teal-5";
    },
    todoInfo() {
      return this.todoitem;
    },
  },
};
</script>




<style>

  @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@500&display=swap');


.text-h5{
font-family: 'Kanit', sans-serif;
font-size: 3.75;
font-weight: 300;
line-height: 3.75rem;
letter-spacing: -0.00833em;
}
.text-h6{
font-size: 3.75;
font-weight: 400;
line-height: 2.75rem;
letter-spacing: -0.00833em;
}



</style>
