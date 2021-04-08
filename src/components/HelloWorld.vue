<template>
  <div class="container mt-5">
    <div >
      <div class="col form-inline">
        <b-form-input
          id="input-2"
          v-model="newTask"
          required
          placeholder="Add List"
          @keyup.enter="add"
        ></b-form-input>
        <b-button @click="add" variant="primary" class="ml-3">Add List</b-button>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col-3" v-for="(ele,index) in list" :key="ele.title">
        <div class="p-2 alert alert-primary">
         <button @click="deleteList(index)" style="float: right;">x</button>
          <h3>{{ele[0].title}}</h3>
          <draggable
            class="list-group kanban-column"
            :list="ele[1]"
            group="tasks"
          >
            <div
              class="list-group-item"
              v-for="element in ele[1]"
              :key="element.name"
            >
             <b-button @click="deleteCard(index,element)" style="float: right;">x</b-button>
              {{ element.title }}
              <br/>
              {{element.dec}}
             
            </div>
          </draggable>
          <button @click="addTask(index)" style="border-radius: 50%;"  v-b-modal.modal-1>+</button>
        </div>
      </div>
    </div>
    <b-modal id="modal-1" title="Add List Card Details" hide-footer ref="my-modal">
   <b-form-input
          v-model="taskTitle"
          required
          placeholder="Add Title"
        ></b-form-input>
         <b-form-input
          v-model="taskDes"
          required
          placeholder="Add Description"
        ></b-form-input>
         <b-button @click="addTaskToList()"  variant="primary" class="ml-3">Add</b-button>
  </b-modal>
  </div>
</template>


<script>
import draggable from "vuedraggable";
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    draggable
  },
  data() {
    return {
      newTask: "",
      taskTitle:"",
      taskDes: "",
      index :0,
      list:[[{title:"Product"},[
        { title: "Code Sign Up Page" ,dec:"small description"},
        { title: "Test Dashboard" ,dec:"small description"},
        { title: "Style Registration" ,dec:"small description"},
        { title: "Help with Designs",dec:"small description" }
      ]],[{title:"Title"},[
        { title: "Code Sign Up Page",dec:"small description" },
      ]]]
    };
  },
   mounted() {
    if (localStorage.name) {
      console.log("hi")
      this.list=JSON.parse(localStorage.getItem('name'));
    }
  },
  beforeUnmounted() {
    localStorage.name = this.list;
  },
  methods: {
    add() {
      if (this.newTask) {
        this.list.push([{title:this.newTask},[
      ]]);
        this.newTask = "";
      }
     localStorage.setItem('name', JSON.stringify(this.list));
    },
    deleteList(value) {
      this.list.splice(value,1);
      localStorage.setItem('name', JSON.stringify(this.list));
    },
    deleteCard(value,ele) {
      console.log(value,ele);
      const c=this.list[value][1].findIndex(((item) => item.title == ele.title));
      this.list[value][1].splice(c,1);
       localStorage.setItem('name', JSON.stringify(this.list));
    },
    addTask(value) {
      this.index=value;
    },
    addTaskToList() {
      if(this.taskDes && this.taskTitle) {
         this.list[this.index][1].push( { title: this.taskTitle,dec:this.taskDes });
      }
     
      this.$refs['my-modal'].hide();
      this.taskTitle="";
      this.taskDes="";
       localStorage.setItem('name', JSON.stringify(this.list));
    }
  }
}
</script>

<style>

.kanban-column {
  min-height: 300px;
}
</style>
