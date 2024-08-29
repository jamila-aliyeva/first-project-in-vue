<script setup>
import { ref } from "vue";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const title = ref("");

const todo = ref([
  // {
  //   id: 1,
  //   title: "title",
  //   isDone: false,
  // },
]);


// Adding task
const addNewTask = () => {
  const task = {
    id: Date.now(),
    title: title.value, 
    isDone: false,
  };

  if(task.title.trim().length==0){
    toast.warning("Please, fill input !", {
        autoClose: 1000,
        "position":"top-center"
      })
  } else{
    todo.value.push(task);
    title.value = ""; 
  }
  
};


//delete task
const deleteTask=(id)=>{
  const tasks=todo.value.filter((el)=>el.id !== id)
  todo.value = tasks
  toast("Task deleted !", {
        autoClose: 1000,
        "position":"top-center"
      })
}


// set status
const setStatus=(id)=>{
 todo.value.forEach(el=>{
  if(el.id== id){
    el.isDone=true
    toast.success("Set status!", {
        autoClose: 1000,
        "position":"top-center"
      })
  }
 })
}

</script>

<template>
  <div class="container">
    <div class="wrapper">
      <form action="#" @submit.prevent="addNewTask">
        <input type="text" placeholder="Enter task title" v-model="title">
        <button type="submit">add task</button>
      </form>

      <ul class="list" v-if="todo.length">
        <li v-for="(el, index) in todo" :key="el.id" class="list__item">
          <strong>{{ index + 1 }}</strong>
          <p :class="[el.isDone ? 'isDone' : '']">{{ el?.title }}</p>

          <button @click="deleteTask(el.id)" class="delete">delete</button>
          <button :disabled="el.isDone" @click="setStatus(el.id)" class="done">done</button>
        </li>
      </ul>

      <div v-else class="content">
        <h3>you don't have tasks</h3>
      </div>
    </div>
  </div>
</template>


<style  scoped>
.content{
  padding: 30px;
  text-align: center;
  color: #0240ab;
}
.isDone{
 text-decoration: line-through;
}
.success{
  color: rgb(19, 68, 51);
}
.denger{
  color: darkred;
}

 .wrapper{
  max-width: 650px;
  min-height: 60vh;
  background-color: aliceblue;
  margin: 50px auto;
  padding: 50px;
  border-radius: 20px;
  border: 5px solid rgb(255, 255, 255);


  form{
    display: flex;
    gap: 10px;
    /* justify-content: space-between; */

    input{
      padding: 10px;
      flex-grow: 2;
      border: 1px solid #3d3d3d;
      border-radius: 10px 0 0 10px;


    }
    button{
      border: none;
      border-radius: 10px;
      padding: 10px;
      background-color: rgb(0, 92, 173);
      color: #fff;
    }
  }

  .list{
    padding: 5px;
    margin: 10px 0;
    /* display: flex; */
    border-radius: 10px;
    .list__item{
      padding: 12px;
      width: 100%;
      display: flex;
      justify-content: space-between;

      button{
        padding-inline:20px;
        border: none;
        border-radius: 5px;
      }
      .delete{
        background-color: red;
        color: #fff;

        
      }
      .delete:focus{
         background-color: #fff;
         color: #3d3d3d;
         border: 1px solid red;
        }
        .done{
          background-color: rgb(0, 92, 173);
          color: #fff;
        }
        .done:focus{
          background-color: #fff;
          color: #3d3d3d;
          border: 1px solid rgb(0, 92, 173);
        }
    }
  }

 }

</style>
