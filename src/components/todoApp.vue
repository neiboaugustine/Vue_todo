<template>
  <div class="container">
    <h1 class="text-center mt-5">My Vue Todo App</h1>

    <!-- inputs -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control" />
      <button @click="submitTask" class="btn btn-warning rounded-7">Submit</button>
    </div>

    <!-- task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">
              {{task.name}}
            </span>
          </td>
          <td style="width: 120px;">
            <span @click="changeStatus(index)" class="pointer" :class="{
              'text-danger': task.status ==='to-do',
              'text-warning': task.status ==='in-progress',
              'text-success': task.status ==='finished'
            }">
            {{firstCharUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center">
              <span data-toggle="modal" :data-target="'#exampleModal'+index" class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Launch demo modal
</button> -->

<!-- Modal -->
<div v-for="(task, index) in tasks" :key="index" class="modal fade" :id="'exampleModal'+index" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Do you want to delete:</h5>
        <!-- <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button> -->
      </div>
      <div class="modal-body">
        <h2>{{task.name}}</h2>
      </div>
      <div class="modal-footer">
        <button :id="'closeModal-'+index" type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
        <button  @click="deleteTask(index)" type="button" class="btn btn-primary">Delete</button>
      </div>
    </div>
  </div>
</div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      task:'',
      editedTask:null,
      availableStatuses:['to-do','in-progress', 'finished'],
      tasks: [
        {
          name: "i will complete this task today",
          status: "finished",
        },
        {
          name: "code for an hour",
          status: "in progress",
        },
        {
          name: "make music",
          status: "in progress",
        },
      ],
    };
  },
  methods:{
    submitTask(){
     if(this.task.length===0)return;

     if(this.editedTask===null){
      this.tasks.push({
      name:this.task,
      status:'to-do'
     })
     }else{
      this.tasks[this.editedTask].name=this.task
      this.editedTask=null
     }
     
     this.task=''
    },
    deleteTask(index){
      this.tasks.splice(index,1);
      document.getElementById('closeModal-'+index).click();
    },
    editTask(index){
      this.task=this.tasks[index].name
      this.editedTask = index
    },
    changeStatus(index){
      let newIndex=this.availableStatuses.indexOf(this.tasks[index].status)
      if(++newIndex>2) newIndex=0
      this.tasks[index].status = this.availableStatuses[newIndex]
    },
    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  }
};
</script>


<style scoped>
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>
