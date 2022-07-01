<script setup>
import { ref } from 'vue';
import BaseButton from '/src/components/BaseButton.vue';
import ButtonAdd from '/src/components/ButtonAdd.vue';
import { useTodoList } from '/src/composables/useTodoList.js';
const todoRef = ref();
const isEditRef = ref(false);
const { todoListRef, add, show, edit, del, check, countFin } = useTodoList();

const inpSetup = document.getElementById('inp');
console.log(inpSetup);

// onMounted(() => {
//   const inpMmount = document.getElementById('inp').value;
//   console.log('inpMmount');
// });

// onUpdated(() => {
//   console.log('onUpdated:', todoRef.value);
// });

const changeCheck = (id) => {
  check(id);
};

const addTodo = () => {
  add(todoRef.value);
  todoRef.value = '';
};

const showTodo = (id) => {
  todoRef.value = show(id);
  isEditRef.value = true;
};

const editTodo = () => {
  edit(todoRef.value);
  isEditRef.value = false;
  todoRef.value = '';
};

const deleteTodo = (id) => {
  del(id);
};

const countFinMethod = () => {
  console.log('method');
  const finArr = todoListRef.value.filter((todo) => todo.checked);
  return finArr.length;
};

const test = () => {
  console.log('test');
};

console.log('setup');
// const todoListRef = ref([
//   { id: 1, task: 'TODO1' },
//   { id: 2, task: 'TODO2' },
//   { id: 3, task: 'TODO3' },
// ]);
// const todoListRef = ref([]);
// const ls = localStorage.todoList;
// todoListRef.value = ls ? JSON.parse(ls) : [];

// const addTodo = () => {
//   //   console.log('addTodo');
//   const id = new Date().getTime();
//   todoListRef.value.push({ id: id, task: todoRef.value });
//   //   localStorage.todoList = todoRef.value;
//   localStorage.todoList = JSON.stringify(todoListRef.value);
//   todoRef.value = '';
// };

// const isEditRef = ref(false);
// let editId = -1;

// const showTodo = (id) => {
//   //   console.log('showTodo:' + id);
//   const todo = todoListRef.value.find((todo) => todo.id === id);
//   //   console.log(todo);
//   todoRef.value = todo.task;
//   isEditRef.value = true;
//   editId = id;
// };

// const editTodo = () => {
//   // 編集対象となるTODOを取得
//   const todo = todoListRef.value.find((todo) => todo.id === editId);

//   // TODOリストから編集対象のインデックスを取得
//   const idx = todoListRef.value.findIndex((todo) => todo.id === editId);

//   // taskを編集後のTODOで置き換え
//   todo.task = todoRef.value;

//   // splice関数でインデックスを元に対象オブジェクトを置き換え
//   todoListRef.value.splice(idx, 1, todo);

//   // ローカルストレージに保存
//   localStorage.todoList = JSON.stringify(todoListRef.value);

//   isEditRef.value = false;
//   editId = -1;
//   todoRef.value = '';
// };

// const deleteTodo = (id) => {
//   //   const todo = todoListRef.value.find((todo) => todo.id === id);
//   //   const idx = todoListRef.value.findIndex((todo) => todo.id === id);

//   const { todo, idx } = useTodoList(id);
//   const delMsg = '「' + todo.task + '」を削除しますか？';
//   if (!confirm(delMsg)) return;

//   todoListRef.value.splice(idx, 1);
//   localStorage.todoList = JSON.stringify(todoListRef.value);
// };

// const todoExample = ref(['example1', 'example2', 'example3']);
</script>

<template>
  <div class="box_input">
    <input
      id="inp"
      type="text"
      class="todo_input"
      v-model="todoRef"
      placeholder=" + TODO を入力"
    />
    <!-- <button class="btn green" @click="editTodo" v-if="isEditRef">変更</button>
    <button class="btn" @click="addTodo" v-else>追加</button> -->
    <BaseButton color="green" @on-click="editTodo" v-if="isEditRef"
      >変更</BaseButton
    >
    <!-- <BaseButton color="brue" @on-click="addTodo" v-else>追加</BaseButton> -->
    <ButtonAdd @add-click="addTodo" v-else>追加</ButtonAdd>
  </div>
  <div class="box_list">
    <div class="todo_list" v-for="todo in todoListRef" :key="todo.id">
      <div class="todo" :class="{ fin: todo.checked }">
        <input
          type="checkbox"
          class="check"
          @change="changeCheck(todo.id)"
          :checked="todo.checked"
        /><label>{{ todo.task }}</label>
      </div>
      <div class="btns">
        <!-- <button class="btn green" @click="showTodo(todo.id)">編</button>
        <button class="btn pink" @click="deleteTodo(todo.id)">削</button> -->
        <BaseButton color="green" @on-click="showTodo(todo.id)">編</BaseButton>
        <BaseButton color="pink" @on-click="deleteTodo(todo.id)">削</BaseButton>
      </div>
    </div>
    <div class="finCount">
      <span>完了：{{ countFin }} 、</span>
      <span>未完了：{{ todoListRef.length - countFin }}</span>
    </div>
  </div>

  <!-- <div v-for="(example, index) in todoExample" :key="index">
    <p>{{ index }}.{{ example }}</p>
  </div> -->
</template>

<style scoped>
.box_input {
  margin-top: 20px;
}

.todo_input {
  width: 300px;
  margin-right: 8px;
  padding: 8px;
  font-size: 18px;
  border: 1px solid #aaa;
  border-radius: 6px;
}

.btn {
  padding: 8px;
  background-color: #03a9f4;
  border-radius: 6px;
  color: #fff;
  text-align: center;
  font-size: 14px;
}
.box_list {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.todo_list {
  display: flex;
  align-items: center;
  gap: 8px;
}

.todo {
  border: 1px solid #ccc;
  border-radius: 6px;
  padding: 12px;
  width: 300px;
}

.check {
  border: 1px solid red;
  transform: scale(1.6);
  margin: 0 16px 2px 6px;
}

.btns {
  display: flex;
  gap: 4px;
}

.green {
  background-color: #00c853;
}

.pink {
  background-color: #ff4081;
}

.fin {
  text-decoration: line-through;
  background-color: #ddd;
  color: #777;
}

.finCount {
  margin-top: 8px;
  font-size: 0.8em;
}
</style>
