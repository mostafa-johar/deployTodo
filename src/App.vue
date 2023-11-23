<template>
  <main class="app">

    <!-- Greeting -->
    <section class="greeting">
      <h1 class="title">
        What's up ,
        <input type="text" v-model="name" name="name" placeholder="Name Here">
      </h1>
    </section>

    <!-- create Todo -->
    <section class="createTodo">
      <h3>Create A ToDo</h3>
      <form class="todoForm" @submit.prevent="addList" id="todoForm">

        <h4>What's on your todo list?</h4>
        <input type="text" placeholder='example : make a video' class="inputcontent" v-model="inputContent">
        <h4>pick a category</h4>

        <section class="options">

          <label for="business">
            <input type="radio" id="business" v-model="input_category" value="business">
            <span class="bubble business"></span>
            <div class="optionName">Business</div>
          </label>

          <label for="personal">
            <input type="radio" id="personal" v-model="input_category" value="personal">
            <span class="bubble personal"></span>
            <div class="optionName">Personal</div>
          </label>
          
        </section>
        <input type="submit" class="addBtn" value="Add todo" />

      </form>

    </section>
    <!-- todo list -->
    <section class="todoList">
      <h3>TODO LIST</h3>
      <div class="list">
        <div class="todoItem business" v-for="out in output">
          <label>
            <input type="checkbox" v-model="out.done">
            <span :class="`bubble ${out.category == 'business' ? 'business' : 'personal'}`"></span>
            <p>{{ out.content }}</p>
          </label>
          <button class="deleteBtn" @click="removeTodo(out)">Delete</button>
        </div>
      </div>
    </section>


  </main>
</template>
<script setup>
/*==========
 == import
===========*/
import { ref, watch, onMounted } from 'vue';

//==== data =====
const name = ref('');
const inputContent = ref('');
const input_category = ref(null);
const output = ref([]);

  // add new task to array
const addList = () => {
  if (inputContent.value.trim() === '' || input_category.value == null) {
    return
  }
  output.value.push(
                    {
                      content: inputContent.value, 
                      category: input_category.value,
                      createdAt: new Date().getTime(), 
                      done : false
                    }
                   )
      inputContent.value = ''
}

// === Remove task from array
const removeTodo = (out) =>{
     output.value = output.value.filter((t)=> t !== out)
}
// ==== Local storage
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(output, (newVal) => {
  localStorage.setItem('todo', JSON.stringify(newVal))
  
},{deep:true})

onMounted(() => {
  name.value = localStorage.getItem('name') ||'' ;
  output.value = JSON.parse(localStorage.getItem('todo')) || []
  
})



</script>
