<template>
  
<div id="TodoApp">
  <h1 :class="{'header-text-light': this.lightmode}">TO-DO</h1>
  <button @click="toggleLightMode" :class="{'light-toggle-light': this.lightmode}" class="light-toggle">Toggle Theme</button>
  <form>
    <input :class="{'form-input-light': this.lightmode}" type="text" placeholder="Enter Task" v-model="task">
    <button @click.prevent="addTask" :class="{'btn-exceeded': getTaskLength > 180}">Add ({{ getTaskLength }}/180)</button>
  </form>
  <table :class="{'table-th-td-light': this.lightmode}">
    <thead>
      <tr>
        <th class="task" :class="[{'table-th-td-light': this.lightmode}, {'th-light': this.lightmode}]">Task</th>
        <th class="status" :class="[{'table-th-td-light': this.lightmode}, {'th-light': this.lightmode}]">Status</th>
        <th class="action" :class="[{'table-th-td-light': this.lightmode}, {'th-light': this.lightmode}]">#</th>
        <th class="action" :class="[{'table-th-td-light': this.lightmode}, {'th-light': this.lightmode}]">#</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td class="task" :class="[{'done-task': task.status === 'Done'}, {'table-th-td-light': this.lightmode}, {'td-light': this.lightmode}]">{{ task.name }}</td>
        <td class="status"  @click="toggleStatus(index)" :class="[{'done-status': task.status === 'Done'}, {'table-th-td-light': this.lightmode}, {'td-light': this.lightmode}]">{{ task.status }}</td>
        <td class="action" :class="[{'table-th-td-light': this.lightmode}, {'td-light': this.lightmode}]"><a class="btn edit-btn" @click="editTask(index)">Edit</a></td>
        <td class="action" :class="[{'table-th-td-light': this.lightmode}, {'td-light': this.lightmode}]"><a class="btn del-btn" @click="removeTask(index)">Remove</a></td>
      </tr>
    </tbody>
  </table>
</div>
  
</template>

<script>

export default {
  name: 'TodoApp',

  data() {
    return {
      lightmode: false,
      task: '',
      editedTask: null,
      tasks: [],
      statuses: [
        'Todo',
        'Done'
      ]
    }
  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem('tasks'))
    this.lightmode = JSON.parse(localStorage.getItem('lightmode'))
    this.setBodyLightMode()
  },
  computed: {
    getTaskLength() {
      return this.task.length
    }
  },
  methods: {
    toggleLightMode() {
      this.lightmode = !this.lightmode
      localStorage.setItem('lightmode', JSON.stringify(this.lightmode))
      this.setBodyLightMode()
    },
    setBodyLightMode() {
      if (document.body.classList.contains('darkmode')) {
        document.body.classList.remove('darkmode')
      }
      if (document.body.classList.contains('lightmode')) {
        document.body.classList.remove('lightmode')
      }
      if (!this.lightmode) {
        document.body.classList.add('darkmode')
      }
      else {
        document.body.classList.add('lightmode')
      }
    },
    addTask() {
      if (this.getTaskLength <= 180) {
        if (this.editedTask === null) {
          if (this.task.length !== 0) {
            this.tasks.push({
              name: this.task,
              status: 'Todo'
            })
          }
        }
        else {
          this.tasks[this.editedTask].name = this.task
          this.editedTask = null
        }
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
        this.task = ''
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    toggleStatus(index) {
      if (this.tasks[index].status === this.statuses[0]) {
        this.tasks[index].status = this.statuses[1]
      }
      else {
        this.tasks[index].status = this.statuses[0]
      }
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}

</script>

<style>

#TodoApp {
  padding: 0 2rem;
  color: #d8d8d8;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

h1 {
  text-align: center;
  padding: 2rem;
}

.header-text-light {
  color: black
}

.light-toggle {
  background-color: #222;
  color: #d8d8d8;
  border: 3px solid #444;
  border-radius: .5rem;
  width: 8rem;
  margin: 0 auto;
  padding: .5rem 0;
}

.light-toggle-light {
  background-color: #999;
  color: black;
  border: 3px solid #777;
}

.light-toggle:hover {
  cursor: pointer;
}

form {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
  border-radius: .5rem;
}

form input {
  width: 77%;
  padding: .5rem 0;
  border: none;
  background-color: #222;
  color: white;
  border-radius: .5rem 0 0 .5rem;
}

.form-input-light {
  background-color: #d1d1d1;
  color: black;
}

form button {
  width: 23%;
  padding: .5rem 0;
  background-color: #32a032;
  color: white;
  border: none;
  border-radius: 0 .5rem .5rem 0;
}

form button:hover {
  background-color: #44a744;
  cursor: pointer;
}

.btn-exceeded, .btn-exceeded:hover {
  background-color: red;
}

table {
  table-layout: fixed;
  width: 100%;
}

table, th, td {
  border-collapse: collapse;
  border: 1px solid #111;
}

.table-th-td-light {
  border: 1px solid #999;
}

th {
  font-weight: bold;
  background-color: #222;
}

.th-light {
  background-color: #cacaca;
  color: black;
}

td {
  font-weight: lighter;
  background-color: #444;
}

.td-light {
  background-color: #e9e9e9;
  color: black;
}

.task, .status {
  text-align: left;
  padding: .5rem 0 .5rem .5rem;
}

.task {
  word-wrap: break-word;
  min-width: 4rem;
}

.done-task {
  text-decoration: line-through;
  color: #999;
}

.status {
  min-width: .5rem;
  cursor: pointer;
  user-select: none;
}

.done-status {
  color: #44a744;
}

.action {
  text-align: center;
  min-width: 4rem;
  user-select: none;
}

.btn {
  text-decoration: none;
  color: #d8d8d8;
  padding: .1rem .25rem;
  border-radius: .5rem;
  user-select: none;
}

.edit-btn {
  background-color: #3a74b6;
}

.edit-btn:hover {
  background-color: #4d7eb6;
  cursor: pointer;
}

.del-btn {
  background-color: #c23a3a;
}

.del-btn:hover {
  background-color: #c45555;
  cursor: pointer;
}

</style>
