<template>
  <div
    v-for="todoItem in todoList"
    :key="todoItem.id"
    class="flex items-center justify-between"
  >
    <div class="flex items-center gap-[4px]">
      <el-checkbox v-model="todoItem.status" />
      <p
        v-if="!todoItem.isEditing"
        :class="[
          'text-[18px]/[18px] font-medium',
          { 'text-green-500': !todoItem.status },
          { 'line-through text-orange-500': todoItem.status },
        ]"
      >
        {{ todoItem.name }}
      </p>
      <el-input v-else v-model="todoItem.name" type="text" />
    </div>
    <div v-if="!todoItem.isEditing" class="flex items-center gap-[8px]">
      <el-button type="warning" @click.prevent="emit('edit', todoItem)">
        Редактировать
      </el-button>
      <el-button type="danger" @click.prevent="emit('delete', todoItem)">
        Удалить
      </el-button>
    </div>
    <div v-else>
      <el-button type="success" @click.prevent="emit('save', todoItem)">
        Сохранить
      </el-button>
      <el-button type="info" @click.prevent="emit('stop', todoItem)">
        Отменить
      </el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { IToDoItem } from "./ToDo.vue";

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
