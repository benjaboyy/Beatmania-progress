<template>
  <div class="container">
    <h2 class="text-center mt-4">Beatmania</h2>
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Search" class="form-control">
      <button @click="submitTasks" class="btn btn-warning rounded-0">Search</button>

    </div>
    <table class="table table-bordered mt-5">
      <thead>
      <tr>
        <td scope="col">Task</td>
        <td scope="col">Status</td>
        <td scope="col" class="text-center">#</td>
        <td scope="col" class="text-center">#</td>
      </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td><span :class="{'finished': task.status === 'finished'}">{{task.name}}</span></td>
          <td><span class="pointer" @click="changeStatus(index)"
                    :class="{'text-danger': task.status === 'to-do',
                    'text-warning': task.status === 'in-progress',
                    'text-success': task.status === 'finished',
            }">
              {{task.status}}
          </span></td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <i class="fa fa-pen"></i>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'helloWorld',
  props: {
    msg: String
  },

  data(){
    return{
      task: '',
      editedTask: null,
      availableStatus: ['to-do','in-progress','finished'],
      tasks: [
        {
          name:'opdracht 1',
          status: 'to-do'
        },
        {
          name:'opdracht 2',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTasks(){
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
    },
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    }
  }
}
</script>

<style>
.pointer{
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>