<template>
  <div class="flex flex-col gap-[12px]">
    <ToDoListItem
      :todoList="todoList"
      @delete="(todoItem) => emit('delete', todoItem)"
      @edit="(todoItem) => emit('edit', todoItem)"
      @save="(todoItem) => emit('save', todoItem)"
      @stop="(todoItem) => emit('stop', todoItem)"
    />
  </div>
</template>

<script setup lang="ts">
import { IToDoItem } from "./ToDo.vue";
import ToDoListItem from "../components/ToDoListItem.vue";

interface Props {
  todoList: IToDoItem[];
}

const { todoList } = defineProps<Props>();

const emit = defineEmits<{
  (e: "delete", todo: IToDoItem): void;
  (e: "edit", todo: IToDoItem): void;
  (e: "save", todo: IToDoItem): void;
  (e: "stop", todo: IToDoItem): void;
}>();
</script>
