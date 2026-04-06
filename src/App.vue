<script setup>
import { ref } from 'vue'
import { useStorage } from '@vueuse/core'
import { computed } from 'vue'
import "tailwindcss";

const items =useStorage('todoList', [])

const todoInput = ref('')

const sortedItems = computed(() => {
  return [...items.value].sort((a, b) => {
    const checkOrder = a.checked - b.checked;
      if (checkOrder !== 0) {
        return checkOrder;
      }
    return b.id - a.id;
  });
});

const A =[1,2,3,];
const B =[...A,4,5,];
//
function addTodo(){
  items.value=[...items.value, { id: Date.now(), text: todoInput.value, checked: false }];
  // items.value.push({ id: Date.now(), text: todoInput.value, checked: false });
  todoInput.value = '';
}

function removeTodo(index){
  items.value = items.value.filter(item => item.id !== index);
}

</script>

<template >
    <div class="container mx-auto px-10 lg:px-20 pt-5 pb-50 bg-white rounded-3xl shadow-lg ">
      <div class="text-4xl lg:text-6xl font-bold text-center my-4 p-2">
        TO DO LIST
      </div>
      <div class="py-6 flex gap-4  ">
        <input v-model="todoInput" placeholder="入力してください" type="text" class="border rounded-md border-gray-300  shadow-md w-full lg:w-110 h-12 px-3" />
        <button :disabled="!todoInput.trim()"  @click="addTodo" class="bg-orange-600 text-white rounded px-4 py-2 w-16 h-12 shadow-md disabled:bg-orange-300 disabled:cursor-not-allowed cursor-pointer">
        add
        </button>
      </div>
      <ul id="list">
        <li v-for="item in sortedItems" :key="item.id">
          <div class="border-b border-gray-300 p-2">
            <fieldset class=" flex items-center cursor-pointer">
              <input v-model="item.checked" type="checkbox" :id="item.id" class=" appearance-none w-6 h-6 rounded-full border-2 border-gray-300 checked:bg-orange-600 checked:border-gray-300"  />
                <label :for="item.id" class="text-lg ml-3" :class="{ 'line-through text-gray-500': item.checked }">{{ item.text }}</label>
              <button @click="removeTodo(item.id)" class="ml-auto text-lg text-gray-400" >
                delete
              </button>
            </fieldset>
          </div>
        </li>
      </ul>
     </div>
</template>