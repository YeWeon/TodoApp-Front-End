<template>
  <div class="inputBox shadow">
      <input type="text" autofocus :value="newTodoItem" @input="handleInput" @keyup.enter="addTodo">
      <span class="addContainer" @click="addTodo">
          <i class="fas fa-plus addBtn"></i>
      </span>
      <MyModal v-if="showModal" @close="showModal = false">
        <template v-slot:header>
          <h3>
            경고!
            <i class="closeModalBtn fas fa-times" @click="showModal = false"></i>
          </h3>
        </template>
        <template v-slot:body>
            <div><i class="fa fa-exclamation-circle closeModalBtn"></i>할 일을 입력해주세요.</div>
        </template>
      </MyModal>
  </div>
</template>

<script lang="ts" setup>
import TodoItem from '@/types/TodoItem';
import MyModal from './common/MyModal.vue'
import { ref } from 'vue'

const showModal = ref(false);
const newTodoItem = ref("")

const emit = defineEmits(["input:todo","add:todo"])

const handleInput = (event: Event) => {
  const todoText = (event.target as HTMLInputElement).value
  if (!todoText) return
  emit("input:todo", todoText)
  newTodoItem.value = todoText
}
const addTodo = () => {
  const todoItem = newTodoItem.value
  if (todoItem !== "") {
      emit('add:todo', todoItem)
      clearInput()
  } else {
    showModal.value = !showModal.value
  }
}
const clearInput = () => {
  newTodoItem.value = ""
}

</script>

<style scoped>
.closeModalBtn {
  color: #42b983;
  margin-right: 10px;
}

i,span {
  cursor: pointer;
}
input:focus {
  outline: none;
}

.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}

.inputBox input {
  border-style: none;
  font-size: 0.9rem;
  width: 80%;
  height: 50%;
}

.addContainer {
  float: right;
  background: linear-gradient(to right, #6478FB, #8763FB);
  display: block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}

.addBtn {
  color: white;
  vertical-align: middle;
}</style>