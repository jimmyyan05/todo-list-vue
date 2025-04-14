<script setup>
import { defineProps, defineEmits } from 'vue'

// 定義組件屬性
const props = defineProps({
  todo: {
    type: Object,
    required: true
  }
})

// 定義事件
const emit = defineEmits(['delete', 'toggle'])

// 處理刪除
function handleDelete() {
  emit('delete', props.todo.id)
}

// 處理狀態切換
function handleToggle() {
  emit('toggle', props.todo.id)
}
</script>

<template>
  <li :data-id="todo.id" class="relative pr-8 group">
    <label class="relative block select-none w-full pl-12 cursor-pointer">
      <input 
        type="checkbox" 
        class="absolute w-full h-full top-0 left-0 m-0 opacity-0 cursor-pointer"
        :checked="todo.checked"
        @change="handleToggle"
      >
      <p 
        class="py-4 border-b border-[#efefef]"
        :class="{ 'text-[#9f9a91] line-through': todo.checked }"
      >
        {{ todo.value }}
      </p>
      
      <!-- 自訂 checkbox 樣式 -->
      <span 
        class="absolute w-5 h-5 border border-[#333] rounded left-4 top-1/2 -translate-y-1/2 transition-opacity"
        :class="{ 'opacity-0': todo.checked }"
      ></span>
      
      <!-- 勾選效果 -->
      <span 
        class="absolute h-3.5 w-2 border-r-2 border-b-2 border-[#ffd370] rotate-45 left-[22px] top-[14px] opacity-0 transition-opacity pointer-events-none"
        :class="{ 'opacity-100': todo.checked }"
      ></span>
    </label>
    
    <!-- 刪除按鈕 -->
    <a 
      href="#" 
      class="absolute text-[#333] right-0 top-1/2 -translate-y-1/2 opacity-0 group-hover:opacity-100 hover:text-red-500"
      @click.prevent="handleDelete"
    >
      <span class="material-icons">close</span>
    </a>
  </li>
</template>