<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col form-inline">
        <b-form-input
          id="input-2"
          v-model="newTask"
          required
          placeholder="Add List"
          @keyup.enter="add"
        ></b-form-input>
        <b-button @click="add" variant="primary" class="ml-3">Add</b-button>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col-3" v-for="(ele,index) in list" :key="ele.title">
        <div class="p-2 alert alert-primary">
         <button @click="deleteList(index)" style="float: right;">x</button>
          <h3>{{ele[0].title}}</h3>
         
          <!-- In Progress draggable component. Pass arrInProgress to list prop -->
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
              {{element.dec}}
             
            </div>
          </draggable>
          
          <button @click="addTask(index,$event)" style="border-radius: 50%;">+</button>
        </div>
      </div>
    </div>
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
      // for new tasks
      newTask: "",
      taskName:"",
      // 4 arrays to keep track of our 4 statuses
      arrBackLog: [
        { title: "Code Sign Up Page" },
        { title: "Test Dashboard" },
        { title: "Style Registration" },
        { title: "Help with Designs" }
      ],
      arrInProgress: [],
      arrTested: [],
      arrDone: [],
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
  methods: {
    //add new tasks method
    add() {
      if (this.newTask) {
        const s=this.newTask
        this.list.push([{title:s},[
      ]]);
        this.newTask = "";
      }
    },
    deleteList(value) {
      this.list.splice(value,1);
    },
    deleteCard(value,ele) {
      console.log(value,ele)
      const c=this.list[value][1].findIndex(((item) => item.name == ele.name))
      console.log(c)
      this.list[value][1].splice(c,1);
    },
    addTask(value,e) {
      console.log(e.target.value)
      this.list[value][1].push( { name: this.taskName });
      this.taskName="";
    }
  }
}
</script>

<style>

.kanban-column {
  min-height: 300px;
}
</style>
