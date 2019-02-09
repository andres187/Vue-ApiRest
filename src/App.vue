<template>
  <div id="app">
    <div class="container">
      
    <h2>Tasks</h2>
      <div class="row">
        <div class="col-sm">
          <div v-if="loading">
            <p>Loading</p>
          </div>
          <div v-else>
            <task
              v-for="item in items"
              :key="item._id"
              :description="item.description"
              :author="item.author"
            >
            </task>
            <br>
          </div>
        </div>
        <div class="col-sm">
          <task-form @save="create">
          </task-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Task from './components/Task';
import TaskForm from './components/TaskForm';
export default {
  name: 'app',
  components: {
    'task': Task,
    'task-form': TaskForm,
  },
  data() {
    return {
      loading: true,
      items: [],
    }
  },
  created() {
    fetch('http://localhost:3000/api/tasks')
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        const { items = [] } = data;
        const tasks = items.map((item) => {
          const { author = {} } = item;
          const { firstname = 'Anonimous', lastname = '' } = author;
          return {
            description: item.description,
            author: `${firstname} ${lastname}`,
          }
        });
        this.items = tasks;
        this.loading = false;
      });
  },
  methods: {
    create(task) {
      fetch('http://localhost:3000/api/tasks', {
        method: 'POST',
        body: JSON.stringify(task),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.items.push(data);
        });
      },
    },
    
};
</script>
