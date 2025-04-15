<template>
  <div>
    <!-- 輸入區域 -->
    <div class="bg-white rounded-lg shadow mb-2 flex p-2 px-5">
      <input
        type="text"
        class="w-full border-0 outline-none p-2"
        placeholder="請輸入待辦事項"
        v-model.trim="newTodo"
        @keyup.enter="addTodo"
      />
      <a
        href="#"
        class="w-10 h-10 bg-[#333] text-white rounded-lg text-2xl flex items-center justify-center hover:bg-[#9f9a91]"
        @click.prevent="addTodo"
        >+</a
      >
    </div>

    <!-- 待辦事項列表 (只有當有資料時才顯示) -->
    <div class="bg-white rounded-lg shadow" v-show="todos.length">
      <!-- 分類標籤 -->
      <ul class="flex text-center text-[#9f9a91]">
        <li
          class="p-4 w-full border-b-2 border-[#efefef] cursor-pointer hover:text-[#333] hover:border-[#333]/30"
          :class="{
            'text-[#333] border-[#333] font-bold': toggleStatus === 'all',
          }"
          @click="changeTab('all')"
        >
          全部
        </li>
        <li
          class="p-4 w-full border-b-2 border-[#efefef] cursor-pointer hover:text-[#333] hover:border-[#333]/30"
          :class="{
            'text-[#333] border-[#333] font-bold': toggleStatus === 'work',
          }"
          @click="changeTab('work')"
        >
          待完成
        </li>
        <li
          class="p-4 w-full border-b-2 border-[#efefef] cursor-pointer hover:text-[#333] hover:border-[#333]/30"
          :class="{
            'text-[#333] border-[#333] font-bold': toggleStatus === 'done',
          }"
          @click="changeTab('done')"
        >
          已完成
        </li>
      </ul>

      <!-- 待辦事項內容 -->
      <div class="p-4 px-5">
        <ul>
          <TodoItem
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @delete="deleteTodo"
            @toggle="toggleTodoStatus"
          />
        </ul>

        <!-- 清單頁腳 -->
        <div class="flex justify-between pt-6 text-sm">
          <p>
            <span>{{ uncompletedCount }}</span> 個待完成項目
          </p>
          <a
            href="#"
            class="text-[#9f9a91] hover:text-red-500"
            @click.prevent="clearCompleted"
            >清除已完成項目</a
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import TodoItem from "./TodoItem.vue";

// 狀態管理
const newTodo = ref(""); // 儲存輸入框的值，初始為空字符串
const todos = ref([]); //儲存所有待辦事項的數組，初始為空數組
const toggleStatus = ref("all"); // 'all', 'work', 'done'

// 計算屬性 - 過濾待辦事項
// 不會直接改變原始數據，而是返回一個新的數組
const filteredTodos = computed(() => {
  if (toggleStatus.value === "all") {
    return todos.value;
  } else if (toggleStatus.value === "work") {
    return todos.value.filter((todo) => !todo.checked);
  } else {
    return todos.value.filter((todo) => todo.checked);
  }
});

// 計算屬性 - 未完成項目數量
const uncompletedCount = computed(() => {
  return todos.value.filter((todo) => !todo.checked).length;
});

// 新增待辦事項
function addTodo() {
  if (newTodo.value.trim() === "") {
    alert("請輸入正確資料");
    return;
  }

  todos.value.push({
    id: Date.now(),
    value: newTodo.value,
    checked: false,
  });

  newTodo.value = "";
}

// 切換分類標籤
function changeTab(status) {
  toggleStatus.value = status;
}

// 刪除單一待辦事項
function deleteTodo(id) {
  if (confirm("確定刪除這個項目?")) {
    todos.value = todos.value.filter((todo) => todo.id !== id);
  }
}

// 切換待辦事項狀態
function toggleTodoStatus(id) {
  const todo = todos.value.find((item) => item.id === id);
  if (todo) {
    todo.checked = !todo.checked;
  }
}

// 清除已完成項目
function clearCompleted() {
  todos.value = todos.value.filter((todo) => !todo.checked);
}
</script>
