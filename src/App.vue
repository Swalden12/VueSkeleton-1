
<script setup>
import { ref, watch } from 'vue';
import "./assets/tailwind.css";

var modalShown = ref(false);

var tasks = ref({
  'Example Task': {
    Title: 'Example Task',
    Notes: 'This is a note about this example task. You can mark it complete by pressing the red X above',
    Completed: false
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

function makeid(length) {
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
function ToggleTaskCompletion(task) {
  task.Completed = !task.Completed;
}
function CreateTask(title, notes) {
  ToggleModal();
  tasks.value[makeid(16)] = {
    "Title": title,
    "Notes": notes,
    "Completed": false
  };
}
function ToggleModal(){
  modalShown.value = !modalShown.value;
}
function DeleteTask(taskId){
  delete tasks.value[taskId];
}
</script>

<template>
  <div v-if="modalShown" class="modal">
    <div className="modal-content">
      <input v-model="newTaskTitle" type="text" placeholder="Task Title"
      className="bold w-full h-10 border-2 p-3 rounded-md">
      <br>
      <textarea v-model="newTaskNotes" placeholder="Task Notes"
        className="bold w-full border-2 p-3 rounded-md resize-none h-96"></textarea>
      <div
      className="flex justify-between mt-auto">
        <button @click="ToggleModal()">Cancel</button>
        <button @click='CreateTask(newTaskTitle,newTaskNotes);newTaskTitle=null;newTaskNotes=null;'>Save</button>
      </div>
    </div>
  </div>
  <h1 style="text-align: center;">
    {{ msg }}
  </h1>
  <div id="tasks-todo" className="flex gap-2 p-10 flex-wrap">
    <div v-for="(item, key) in tasks">
      <div className="bg-aliceblue pt-5 p-5 w-[400px] bg-slate-100 rounded-md shadow-2xl">
        <div className="flex justify-between w-full">
          <h3 className="self-center font-bold text-xl">{{ item.Title }}</h3>
          <div className="flex items-center">
            <p>Status: </p>
            <svg style="cursor: pointer; user-select: none;" v-if="item.Completed" v-on:click="ToggleTaskCompletion(item)"
              viewBox="0 0 24 24" height="60px" width="60px" fill="none" stroke="green"
              xmlns="http://www.w3.org/2000/svg">
              <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
              <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
              <g id="SVGRepo_iconCarrier">
                <g id="Interface / Check">
                  <path id="Vector" d="M6 12L10.2426 16.2426L18.727 7.75732" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round"></path>
                </g>
              </g>
            </svg>
            <svg style="cursor: pointer; user-select: none;" v-else="item.Completed" v-on:click="ToggleTaskCompletion(item)" viewBox="0 0 24 24" height="60px" width="60px" fill="none"
              stroke="red" xmlns="http://www.w3.org/2000/svg">
              <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
              <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
              <g id="SVGRepo_iconCarrier">
                <g id="Menu / Close_SM">
                  <path id="Vector" d="M16 16L12 12M12 12L8 8M12 12L16 8M12 12L8 16" stroke-width="2"
                    stroke-linecap="round" stroke-linejoin="round"></path>
                </g>
              </g>
            </svg>
            <svg style="cursor: pointer; user-select: none;" v-on:click="DeleteTask(key)" width="30px" height="30px"
              viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
              <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
              <g id="SVGRepo_iconCarrier">
                <g id="Interface / Trash_Full">
                  <path id="Vector"
                    d="M14 10V17M10 10V17M6 6V17.8C6 18.9201 6 19.4798 6.21799 19.9076C6.40973 20.2839 6.71547 20.5905 7.0918 20.7822C7.5192 21 8.07899 21 9.19691 21H14.8031C15.921 21 16.48 21 16.9074 20.7822C17.2837 20.5905 17.5905 20.2839 17.7822 19.9076C18 19.4802 18 18.921 18 17.8031V6M6 6H8M6 6H4M8 6H16M8 6C8 5.06812 8 4.60241 8.15224 4.23486C8.35523 3.74481 8.74432 3.35523 9.23438 3.15224C9.60192 3 10.0681 3 11 3H13C13.9319 3 14.3978 3 14.7654 3.15224C15.2554 3.35523 15.6447 3.74481 15.8477 4.23486C15.9999 4.6024 16 5.06812 16 6M16 6H18M18 6H20"
                    stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                </g>
              </g>
            </svg>
          </div>
        </div>
        <p className="break-words">{{ item.Notes }}</p>
      </div>
    </div>
    <button @click="ToggleModal()"
    className="border-none font-bold cursor-pointer w-[400px] bg-aliceblue rounded-10 p-10 bg-slate-50 rounded-md shadow-2xl">Add
      Task</button>
  </div>
</template>
