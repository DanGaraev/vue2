<template>
  <div class="w-[100vw] h-[100vh] flex items-center justify-center">
    <div
      class="w-[500px] h-[800px] bg-gray-600 py-[14px] px-[20px] flex flex-col gap-[12px]"
    >
      <div>
        <p class="text-[24px]/[24px] font-bold">ToDo List</p>
      </div>
      <form class="flex items-center gap-[8px]">
        <el-input v-model="creatingToDo" class="create_input" type="text" />
        <el-button
          type="primary"
          @click.prevent="handleCreateToDo(creatingToDo)"
        >
          Завести задачу
        </el-button>
      </form>
      <ToDoList
        :todoList="todoList"
        @delete="(todoItem) => handleDeleteToDo(todoItem)"
        @edit="(todoItem) => handleEditToDo(todoItem)"
        @save="(todoItem) => handleSaveEdited(todoItem)"
        @stop="(todoItem) => handleStopEdit(todoItem)"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import ToDoList from "../components/ToDoList.vue";

const creatingToDo = ref("");

const handleCreateToDo = (creatingToDo: string) => {
  todoList.value.push({
    id: nextToDoID.value,
    status: false,
    name: creatingToDo,
    isEditing: false,
  });
  nextToDoID.value += 1;
};

const nextToDoID = ref(1);
export interface IToDoItem {
  id: number;
  status: boolean;
  name: string;
  isEditing: boolean;
}
const todoList = ref<IToDoItem[]>([
  {
    id: 0,
    status: false,
    name: "Название задачи",
    isEditing: false,
  },
]);

const handleDeleteToDo = (todoItem: IToDoItem) => {
  let todoToDeleteIndex = todoList.value.indexOf(todoItem);
  if (todoToDeleteIndex !== -1) {
    todoList.value.splice(todoToDeleteIndex, 1);
  }
};

const editingToDos = ref<IToDoItem[]>([]);
const handleEditToDo = (todoItem: IToDoItem) => {
  editingToDos.value.push(Object.assign({}, todoItem));
  todoItem.isEditing = true;
};

const handleStopEdit = (todoItem: IToDoItem) => {
  let editingToDo = editingToDos.value.find(
    (editingToDo) => editingToDo.id === todoItem.id
  );
  if (editingToDo) {
    todoItem.name = editingToDo.name;
    todoItem.isEditing = false;
    let editingToDoIndex = editingToDos.value.indexOf(editingToDo);
    editingToDos.value.splice(editingToDoIndex, 1);
  }
};

const handleSaveEdited = (todoItem: IToDoItem) => {
  let editingToDo = editingToDos.value.find(
    (editingToDo) => editingToDo.id === todoItem.id
  );
  if (editingToDo) {
    todoItem.isEditing = false;
    let editingToDoIndex = editingToDos.value.indexOf(editingToDo);
    editingToDos.value.splice(editingToDoIndex, 1);
  }
};
</script>
