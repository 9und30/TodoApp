<template>
  
<div id="TodoApp">
  <h1>TO-DO</h1>
  <form>
    <input type="text" placeholder="Enter Task" v-model="task">
    <button @click.prevent="addTask" :class="{'btn-exceeded': getTaskLength > 180}">Add ({{ getTaskLength }}/180)</button>
  </form>
  <table>
    <thead>
      <tr>
        <th class="task">Task</th>
        <th class="status">Status</th>
        <th class="action">#</th>
        <th class="action">#</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td class="task" :class="{'done-task': task.status === 'Done'}">{{ task.name }}</td>
        <td class="status" @click="toggleStatus(index)" :class="{'done-status': task.status === 'Done'}">{{ task.status }}</td>
        <td class="action"><a class="btn edit-btn" @click="editTask(index)">Edit</a></td>
        <td class="action"><a class="btn del-btn" @click="removeTask(index)">Remove</a></td>
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
      task: '',
      editedTask: null,
      tasks: [],
      statuses: [
        'Todo',
        'Done'
      ]
    }
  },
  computed: {
    getTaskLength() {
      return this.task.length
    }
  },
  methods: {
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
        this.task = ''
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },
    toggleStatus(index) {
      if (this.tasks[index].status === this.statuses[0]) {
        this.tasks[index].status = this.statuses[1]
      }
      else {
        this.tasks[index].status = this.statuses[0]
      }
    }
  }
}

</script>

<style>

#TodoApp {
  width: 100vw;
  padding: 0 2rem;
}

h1 {
  text-align: center;
  padding: 2rem;
}

form {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
  border: 1px solid #999;
}

form input {
  width: 77%;
  padding: .5rem 0;
  border: none;
}

form button {
  width: 23%;
  padding: .5rem 0;
  background-color: #32a032;
  color: white;
  border: none;
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
  border: 1px solid #999;
}

th {
  font-weight: bold;
}

td {
  font-weight: lighter;
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
  color: white;
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
