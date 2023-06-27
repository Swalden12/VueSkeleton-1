<script setup>
import { ref, watch } from 'vue';
import "./assets/tailwind.css";

import checkmarkImg from './assets/img/check.svg';
import xImg from './assets/img/x.svg';
import trashImg from './assets/img/trash.svg';

var modalShown = ref(false);

var tasks = ref({
  'exampleTask': {
    title: 'Example Task',
    notes: 'This is a note about this example task. You can mark it complete by pressing the red X above',
    completed: false
  }
});

const msg = 'Todo App';

var tasksJson = localStorage.getItem("tasks");
if (tasksJson !== null) {
  tasks = ref(JSON.parse(tasksJson));
}

watch(tasks, (newTasks) => {
  localStorage.setItem('tasks', JSON.stringify(newTasks));
}, { deep: true });

function makeId(length) {
  let result = '';
  const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  const charactersLength = characters.length;
  let counter = 0;
  while (counter < length) {
    result += characters.charAt(Math.floor(Math.random() * charactersLength));
    counter += 1;
  }
  return result;
}
function toggleTaskCompletion(task) {
  task.completed = !task.completed;
}
function createTask(title, notes) {
  toggleModal();
  tasks.value[makeId(16)] = {
    title: title,
    notes: notes,
    completed: false
  };
}
function toggleModal(){
  modalShown.value = !modalShown.value;
}
function deleteTask(taskId){
  delete tasks.value[taskId];
}
</script>

<template>
  <div v-if="modalShown" class="modal">
    <div class="modal-content">
      <input v-model="newTaskTitle" type="text" placeholder="Task Title"
      class="bold w-full h-10 border-2 p-3 rounded-md">
      <br>
      <textarea v-model="newTaskNotes" placeholder="Task Notes"
        class="bold w-full border-2 p-3 rounded-md resize-none h-96"></textarea>
      <div
      class="flex justify-between mt-auto">
        <button @click="toggleModal()">Cancel</button>
        <button @click='createTask(newTaskTitle,newTaskNotes);newTaskTitle=null;newTaskNotes=null;'>Save</button>
      </div>
    </div>
  </div>
  <h1 style="text-align: center;">
    {{ msg }}
  </h1>
  <div id="tasks-todo" class="flex gap-2 p-10 flex-wrap">
    <div v-for="(item, key) in tasks" :key="key">
      <div class="bg-aliceblue pt-5 p-5 w-[400px] bg-slate-100 rounded-md shadow-2xl">
        <div class="flex justify-between w-full">
          <h3 class="self-center font-bold text-xl">{{ item.title }}</h3>
          <div class="flex items-center">
            <p>Status: </p>
            <img :src="checkmarkImg" style="cursor: pointer; user-select: none;height: 60px; width: 60px;" v-if="item.completed" @click="toggleTaskCompletion(item)">
            <img :src="xImg" style="cursor: pointer; user-select: none;height: 60px; width: 60px;" v-else @click="toggleTaskCompletion(item)">
            <img :src="trashImg" style="cursor: pointer; user-select: none;height: 30px; width: 30px;" @click="deleteTask(key)">
          </div>
        </div>
        <p class="break-words">{{ item.notes }}</p>
      </div>
    </div>
    <button @click="toggleModal()"
    class="border-none font-bold cursor-pointer w-[400px] bg-aliceblue rounded-10 p-10 bg-slate-50 rounded-md shadow-2xl">Add
      Task</button>
  </div>
</template>
