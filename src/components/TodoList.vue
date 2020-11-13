<template>
	<div class="nav">
		Simple Todo List App
		<transition name="todo" mode="out-in">
			<button @click="toggleTodo('addTodo')" v-if="toggleMode === 'todoList'">
				Add Todo
			</button>
			<button @click="toggleTodo('todoList')" v-else>Todo List</button>
		</transition>
	</div>
	<transition name="todo" mode="out-in">
		<div class="container" v-if="toggleMode === 'todoList'">
			<div v-if="todosLength">
				<ul>
					<li class="todo-heading">
						<div class="todo-content-header">Name</div>
						<div class="todo-content-header">Actions</div>
					</li>
				</ul>
				<ul v-for="tod in todos" :key="tod.id">
					<li>
						<div :class="{ completed: tod.isCompleted }">{{ tod.name }}</div>
						<div class="action">
							<input
								type="checkbox"
								:id="tod.id"
								:name="'completed-' + tod.id"
								class="checkbox"
								checked
								v-if="tod.isCompleted"
								@click="toggleChecked(tod.id)"
							/>
							<input
								type="checkbox"
								name=""
								id=""
								v-else
								class="checkbox"
								@click="toggleChecked(tod.id)"
							/>
							<button class="info" @click="confirmUpdate(tod)">Edit</button>
							<button class="danger" @click="confirmDelete(tod)">Delete</button>
						</div>
						<teleport to="body">
							<base-modal
								:modalIsVisible="modalVisible"
								v-if="modalVisible && editingId === tod.id"
							>
								<template #header> Edit Todo List </template>
								<template #default>
									<label for="name">Enter New Todo Name</label>
									<input type="text" name="" id="name" v-model="form.name" />
									<div class="checkbox-area">
										<input
											type="checkbox"
											name=""
											id=""
											v-model="form.isCompleted"
										/>
										Completed
									</div>
								</template>
								<template #footer>
									<button class="success" @click="updateTodo(tod)">
										Submit
									</button>
									<button class="danger" @click="toggleModal(tod)">
										Cancel
									</button>
								</template>
							</base-modal>
						</teleport>
						<teleport to="body">
							<base-modal
								:modalIsVisible="modalVisible"
								v-if="modalVisible && deletingId === tod.id"
							>
								<template #header> Delete Confirmation </template>
								<template #default>
									Are you sure to delete {{ tod.name }}?
								</template>
								<template #footer>
									<button class="danger" @click="deleteTodo(tod)">
										Delete
									</button>
									<button class="warning" @click="toggleModal(tod)">
										Go Back
									</button>
								</template>
							</base-modal>
						</teleport>
					</li>
				</ul>
			</div>
			<div v-else>
				<h5>Nothing in todos to display!</h5>
			</div>
		</div>

		<div class="container" v-else>
			<div class="form-control">
				<form @submit.prevent="addTodo">
					<input
						v-model="todo.name"
						type="text"
						placeholder="Enter Todo Name"
					/>
					<button type="submit">Add</button>
				</form>
			</div>
		</div>
	</transition>
</template>

<script>
	import { computed, reactive, ref } from "vue";
	import BaseModal from "./BaseModal.vue";
	export default {
		name: "TodoList",
		components: {
			BaseModal,
		},
		setup(_, ctx) {
			const todos = ref([]);
			const toggleMode = ref("todoList");
			const todosLength = computed(() => todos.value.length);
			const modalVisible = ref(false);
			const editingId = ref(null);
			const deletingId = ref(null);
			const form = reactive({
				id: null,
				name: "",
				isCompleted: false,
			});
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

			function updateTodo(todo) {
				if (todo.name == form.name && todo.isCompleted == form.isCompleted) {
					alert("Please, Enter Some Changes!");
				} else {
					todos.value = todos.value.map((v) => {
						if (v.id == form.id) {
							return {
								id: form.id,
								name: form.name,
								isCompleted: form.isCompleted,
							};
						}

						return v;
					});

					toggleModal(form);
				}
			}

			function deleteTodo(todo) {
				todos.value = todos.value.filter((val) => val.id !== todo.id);
				toggleModal(todo)
			}

			function confirmDelete(todo) {
				modalVisible.value = true;
				deletingId.value = todo.id;
			}

			function confirmUpdate(todo) {
				modalVisible.value = true;
				editingId.value = todo.id;
				form.id = todo.id;
				form.name = todo.name;
				form.isCompleted = todo.isCompleted;
			}

			function toggleChecked(id) {
				todos.value = todos.value.map((todo) => {
					if (todo.id == id) {
						return {
							name: todo.name,
							id: todo.id,
							isCompleted: !todo.isCompleted,
						};
					}

					return todo;
				});
			}

			function toggleModal(todo) {
				modalVisible.value = !modalVisible.value;
				editingId.value = null;
				deletingId.value = null;
			}

			return {
				todos,
				form,
				todo,
				todosLength,
				toggleTodo,
				toggleMode,
				addTodo,
				confirmUpdate,
				updateTodo,
				confirmDelete,
				deleteTodo,
				toggleChecked,
				modalVisible,
				editingId,
				deletingId,
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

	.todo-heading {
		border: none;
		padding: 0;
		margin: 0;
	}

	.todo-heading .todo-content-header {
		font-weight: bold;
		font-size: 1rem;
	}

	.todo-enter-from {
		opacity: 0;
		transform: translateX(-100px);
	}
	.todo-enter-active {
		transition: all 0.3s ease-in;
	}
	.todo-enter-to {
		opacity: 1;
		transform: translateX(0);
	}

	.todo-leave-from {
		opacity: 1;
		transform: translateX(0);
	}
	.todo-leave-active {
		transition: all 0.3s linear;
	}
	.todo-leave-to {
		opacity: 0;
		transform: translateX(-100px);
	}
</style>
