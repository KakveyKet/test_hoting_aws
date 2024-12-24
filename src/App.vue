<template>
  <div
    class="bg-teal-500 overflow-auto border h-[25rem]  rounded-xl w-[80%] md:w-[60%] lg:w-[40%] p-4 absolute  top-20 translate-x-1/2">
    <h1 class="text-lg font-bold mb-2">Todo List</h1>
    <div>
      <input v-model="titleIput" class="bg-transparent border placeholder:text-white px-5 py-1 outline-none rounded-lg"
        type="text" placeholder="title...">
      <button @click="addTodo"
        class="bg-gradient-to-l from-indigo-500 via-purple-500 to-pink-500 px-2 py-1 rounded-lg ml-2 ">Add </button>
    </div>
    <div v-for="todo in data">
    <div
      class="border px-5 py-1 rounded-lg mt-2 cursor-pointer hover:bg-gray-800 transition-all justify-between flex items-center ">
      <div v-if="todo.doned">
            <s class="text-red-500">{{todo.title}}</s>
      </div>
      <div v-else>
        <h1 class="text-gray-500">{{todo.title}}</h1>
      </div>
      <div class="flex gap-2">
        <button @click="()=> makeItDone(todo.title)" class="bg-gradient-to-l from-indigo-500 via-purple-500 to-pink-500 px-2 py-1 rounded-lg">Done</button>
        <button @click="()=> deleteTodo(todo.title)" class="bg-red-500 px-2 py-1 rounded-lg">Delete</button>
      </div>

    </div>
    </div>

  </div>
</template>
<script>
import { ref } from 'vue';
export default {
  setup() {
    // add 
    const data = ref();
    const getAll = () =>{
      data.value = JSON.parse(localStorage.getItem('todo'))
    }
   getAll()
    const titleIput = ref("");
    const addTodo = () => {
      if (!titleIput.value) return alert("Please Input The Fill")
      const items = JSON.parse(localStorage.getItem("todo"))
      const existed = items?.find(item => item.title === titleIput.value) 
      if (existed) {
        alert("Already Existed")
        return
      }
      if(items){
        localStorage.setItem('todo', JSON.stringify([...items, {title: titleIput.value, doned: false}]))
      } else {
        localStorage.setItem('todo', JSON.stringify([{title: titleIput.value, doned: false}]))
      }
      titleIput.value = "";
      getAll();
    }
    // delete
    const deleteTodo = (title) => {
      const deleted = data.value.filter(item => item.title !== title);
      localStorage.setItem('todo', JSON.stringify(deleted))
      getAll();
    }
    // make it done
    const makeItDone = (title) => {
      const updated = data.value.map(item => item.title === title ? {...item, doned: true} : item);
      localStorage.setItem('todo', JSON.stringify(updated))
      getAll();
    }
    return {
      data,
      titleIput,
      addTodo,
      deleteTodo,
      makeItDone
    }
  }
}
</script>