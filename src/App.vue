<template>
  <img id = "img" alt="Vue logo" src="./assets/logo.png">
  <div class = "container pt-5">
    <div class = "row">
      <div class = "header col-12">
        <h2>Todo-list</h2>
      </div>
      <div class = "col-10 ps-0 pe-0">
        <input 
          v-model="input"
          placeholder = "To-Do"
          class = "form-control"
        >
      </div>
      <div class = "col-2 pe-0 ps-1">
        <button 
          @click = "addToDo()" 
          class = "btn btn-primary w-100"
        >Add</button>
      </div>
      <div class = "col-12 pt-3 text-start">
        <h3>List To-Do</h3>
      </div>
      {{ToDos}}
      <div 
        v-for="(toDo, index) in toDos"
        :key = "index"
        :class = "'block col-12 row text-start '+(index % 2 !== 0 ? 'second-block ' : '')+(toDo.deleted ? ' deleted ' : '')"
      >
        <div class = "col-10 ps-0 pe-0"><p :class = "'title-todo '+(toDo.checked ? 'checked ' : '')">{{toDo.value}}</p><p class = "data-format">{{toDo.data}}</p></div>
        <div class = "col-1 pe-0 ps-1 text-center m-auto"><input @click="checked(index)" type="checkbox"></div>
        <div class = "col-1 pe-0 ps-1 text-center m-auto"><button @click="deleteTodo(index)" class = "btn btn-primary">{{ toDo.deleted ? "-" : "x" }}</button></div>
      </div>
      <div class = "text-start ps-0 pt-3">Count ToDo : {{countTodo}} {{doneCount > 0 ? ' | Count done : '+doneCount : ''}} {{deleted > 0 ? "| Deleted ToDo :"+deleted : ''}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      toDos: [],
      input: null,
      countTodo: 0,
      doneCount: 0,
      deleted: 0
    }
  },
  methods: {
    addToDo() {
      if(this.input != null && this.input != "") {
        let data = new Date();
        let finalDate = data.getFullYear()+"-"+(data.getMonth()+1)+"-"+data.getDate()+" "+data.getHours()+":"+data.getMinutes();
        this.toDos.push({
          value: this.input,
          data: finalDate,
          checked: false,
          deleted: false
        });
        this.countTodo++;
      } else
        alert("Поле пустое")
    },
    checked(index) {
      this.toDos[index].checked = this.toDos[index].checked ? false : true
    },
    deleteTodo (index) {
      let getTodo = this.toDos[index];
      getTodo.deleted = getTodo.deleted ? false : true;
      if(getTodo.deleted)
        this.deleted++;
      else
        this.deleted--;
      this.toDos.splice(index,1);
      this.toDos.push(getTodo);
    }
  },
  updated() {
    this.countTodo = this.toDos.length;

    let done = 0;
    this.toDos.map(e => {
      if (e.checked && e.deleted !== true)
        done++;
    });
    this.doneCount = done;
  },
  created() {
    this.toDos.push({
          value: "toDo1",
          data: "2021-07-01 15:23",
          checked: false,
          deleted: false
        })
    this.toDos.push({
          value: "toDo2",
          data: "2021-07-01 15:23",
          checked: false,
          deleted: false
        })
    this.countTodo = this.toDos.length
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#img {
  width:2%;
}

.header {
  padding:1rem 0;
}

.block {
  padding:.5rem 1rem;
}

.block p {
  margin:0;
  padding:0;
}

.data-format {
  font-size:.7rem;
  color:grey;
}

.title-todo {
  font-size:1.5rem;
}

.second-block {
  background-color:rgb(242, 242, 242);
}

.checked {
  text-decoration: line-through;
}

.deleted {
  opacity:0.5;
}

@import '~bootstrap/dist/css/bootstrap.css'
</style>
