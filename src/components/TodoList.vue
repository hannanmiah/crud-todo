<template>
  <div class="nav">
    Simple Todo List App
    <button @click="toggleTodo('addTodo')" v-if="toggleMode === 'todoList'">
      Add Todo
    </button>
    <button @click="toggleTodo('todoList')" v-if="toggleMode === 'addTodo'">
      Todo List
    </button>
  </div>
  <div class="container" v-if="toggleMode === 'todoList' && todosLength">
    <ul v-for="tod in todos" :key="tod.id">
      <li>
        <div :class="{ completed: tod.isCompleted }">{{ tod.name }}</div>
        <div class="action">
          <input
            type="checkbox"
            :id="tod.id"
            :name="'completed-' + tod.id"
            v-model="todo.isCompleted"
            class="checkbox"
          />
          <button class="info" @click="toggleModal(tod)">Edit</button>
          <button class="danger" @click="deleteTodo(tod.id)">Delete</button>
        </div>
        <teleport to="body">
          <div v-if="modalVisible && editingId === tod.id">
          <base-modal>
            <template #header>
              Edit Todo List
            </template>
            <template #default>
              <label for="name">Enter New Todo Name</label>
              <input type="text" name="" id="name" v-model="form.name">
              <div class="checkbox-area">
                <input type="checkbox" name="" id=""> Completed
              </div>
            </template>
            <template #footer>
              <button class="success">Submit</button>
              <button class="danger" @click="toggleModal(tod)">Cancel</button>
            </template>
          </base-modal>
        </div>
        </teleport>
      </li>
    </ul>
  </div>
  <div class="container" v-if="toggleMode === 'addTodo'">
    <div class="form-control">
      <form @submit.prevent="addTodo">
        <input v-model="todo.name" type="text" placeholder="Enter Todo Name" />
        <button type="submit">Add</button>
      </form>
    </div>
  </div>
  <div class="container" v-if="!todosLength && toggleMode === 'todoList'">
    <h5>Nothing in todos</h5>
  </div>
</template>

<script>
import { computed, reactive, ref } from "vue";
import BaseModal from "./BaseModal.vue";
export default {
  name: "TodoList",
  components: {
    BaseModal,
  },
  setup(_,ctx) {
    const todos = ref([]);
    const toggleMode = ref("todoList");
    const todosLength = computed(() => todos.value.length);
    const modalVisible = ref(false);
    const editingId = ref(null)
    const form = reactive({
      id: null,
      name: '',
      isCompleted: false
    })
    const todo = reactive({
      name: "",
      isCompleted: false,
    });
    function toggleTodo(mode) {
      toggleMode.value = mode;
    }

    function addTodo() {
      if (todo.name) {
        todos.value.push({
          id: todosLength.value + 1,
          name: todo.name,
          isCompleted: todo.isCompleted,
        });
        todo.name = "";
        todo.isCompleted = false;
        toggleTodo("todoList");
      } else {
        alert("Please, enter a name!");
      }
    }

    function deleteTodo(id) {
      todos.value = todos.value.filter((val) => val.id !== id);
    }

    function toggleModal(todo) {
      modalVisible.value = !modalVisible.value;
      editingId.value = todo.id
      form.id = todo.id
      form.name = todo.name
      form.isCompleted = todo.isCompleted
    }

    return {
      todos,
      form,
      todosLength,
      toggleTodo,
      toggleMode,
      addTodo,
      deleteTodo,
      todo,
      modalVisible,
      editingId,
      toggleModal,
    };
  },
};
</script>
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
ul li {
  display: grid;
  grid-template-columns: 1fr 1fr;
  border: 1px solid #ccc;
  padding: 10px;
}
.completed {
  text-decoration: line-through;
}
ul li .action {
  display: flex;
  justify-content: center;
}
button[type="submit"] {
  margin: 0 5px;
}

input {
  border: 1px solid #ddd;
  padding: 5px;
}

label {
  display: block;
  margin: 5px;
}

.checkbox {
  padding: 10px;
  border: 2px solid rgb(31, 243, 84);
}

.checkbox-area {
  margin: 5px;
  color: green;
}

input:checked {
  background: green;
}

button {
  margin-left: 5px;
  border-radius: 5px;
}
</style>
