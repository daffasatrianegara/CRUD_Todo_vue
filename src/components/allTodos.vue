<template>
  <div class="mt-2">
    <form @submit.prevent="add">
      <input type="hidden" v-model="form.id" />
      <label class="me-2" for="">Todo</label>
      <input class="" type="text" v-model="form.todo" />
      <label class="ms-3 me-2" for="">deskripsi </label>
      <input type="text" v-model="form.description" />
      <button class="btn btn-success ms-3" type="submit" v-show="!updateSubmit">
        add
      </button>
      <button
        class="btn btn-success ms-3"
        type="button"
        v-show="updateSubmit"
        @click="update(form)"
      >
        Update
      </button>
    </form>
    <div class="card w-100 h-auto m-2" v-for="todo in todos" :key="todo.id">
      <div class="d-flex flex-row">
        <div class="card-body m-1 p-1">
          <div class="card-title m-0 p-0">
            <p class="fs fs-4 m-0 p-0 fw-bold">{{ todo.todo }}</p>
          </div>
          <p class="m-0 p-0 fs-5">{{ todo.description }}</p>
        </div>
        <div class="me-2 mt-2">
          <button class="btn btn-warning me-2" @click="edit(todo)">
            Update Data
          </button>
          <button class="btn btn-danger" @click="del(todo)">Delete Data</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";

export default {
  name: "all_data_todos",
  data() {
    return {
      todos: [],
      updateSubmit: false,
      form: {
        id: "",
        todo: "",
        description: "",
      },
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/todos")
        .then((res) => {
          this.todos = res.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    add() {
      axios.post("http://localhost:3000/todos/", this.form).then((res) => {
        this.load();
        this.form.todo = "";
        this.form.description = "";
      });
    },
    edit(todo) {
      this.updateSubmit = true;
      this.form.id = todo.id;
      this.form.todo = todo.todo;
      this.form.description = todo.description;
    },
    update(form) {
      axios.put(`http://localhost:3000/todos/${form.id}`, {
        todo: this.form.todo,
        description: this.form.description,
      }).then(res => {
        this.load()
        this.form.id = "";
        this.form.todo = "";
        this.form.description = "";
        this.updateSubmit = false;
        
      }).catch((error) => {
        console.log(error);
      });
    },
    del(todo) {
      axios.delete(`http://localhost:3000/todos/${todo.id}`).then(res => {
        this.load()
      })
    }
  },
};
</script>
