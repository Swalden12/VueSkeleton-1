<template>
  <div class="container">
    <div class="todo">
      <h2>To-Do List</h2>
      <div class="row">
        <input v-model="newTask" @keypress.enter="addTask" type="text" id="input-box" placeholder="Add your text">
        <button @click="addTask">Add</button>
      </div>
      <ul id="list-container">
        <li v-for="(task, index) in tasks" :key="index" :class="{ checked: task.checked }" @click="toggleTask(task)">
          {{ task.text }}
          <span @click.stop="removeTask(index)">✗</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: JSON.parse(localStorage.getItem('data')) || []
    };
  },
  methods: {
    addTask() {
      if (this.newTask === '') {
        alert('Write something!');
        return;
      }
      this.tasks.push({ text: this.newTask, checked: false });
      this.newTask = '';
      this.saveData();
    },
    toggleTask(task) {
      task.checked = !task.checked;
      this.saveData();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveData();
    },
    saveData() {
      localStorage.setItem('data', JSON.stringify(this.tasks));
    }
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
}
.container {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #153677, #4e085f);
  padding: 10px;
}
.todo {
  width: 100%;
  max-width: 540px;
  background: #fff;
  margin: 100px auto 20px;
  padding: 40px 30px 70px;
  border-radius: 10px;
}
.todo h2 {
  color: #002765;
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}
.row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #edeef0;
  border-radius: 30px;
  padding-left: 20px;
  margin-bottom: 25px;
}
input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  padding: 10px;
  font-size: 14px;
}
button {
  border: none;
  outline: none;
  padding: 16px 50px;
  background: #ff5945;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  border-radius: 40px;
}
ul li {
  list-style: none;
  font-size: 17px;
  padding: 12px 8px 12px 50px;
  user-select: none;
  cursor: pointer;
  position: relative;
}
ul li.checked {
  color: #555;
  text-decoration: line-through;
}
ul li span {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
}
</style>
