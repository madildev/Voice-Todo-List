<template>
  <div id="app">
      <div class="header">
        <h1>{{title}}</h1>
      </div>
      <vue-speech @onTranscriptionEnd = "onEnd" v-show="show"/>
      <div class="container">
        <div class="add-element">
          <div class="element-input">
           <form @submit.prevent="addTodo">
           <input v-model="newTodo" type="text" name="newTodo" id="newTodo" value="" placeholder="What you want to do next!" required>
            <button type="submit" name="button" onclick="onDisplay">Add</button>
            <button @click="allDone" type="button" name="button">All Done</button>
           </form>
          </div> 
        </div>
        <div class="element-list">
          <ul>
            <li v-for="(todo,index) in todos" :key="index">
                <input type="text" name="newTodo" id="Output" :class="{done: todo.done}" v-model="todo.title" disabled>
            
                <button @click="toggle(todo)" type="button" name="button">Done</button>
                <button @click="removeTodo(todo)" type="button" name="button">Remove</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>


<script>
export default {
  name: 'App',
  data(){
    return {
         title: 'Todo List',
         newTodo: '',
         todos: [],
         show: false
    }
  },
   Mounted()
   {
     this.loadHistory();
   },
    methods: {
      //This function takes the sentnece spoken by the user
      onEnd({lastSentence}){
          this.newTodo = lastSentence;
          this.addTodo();
      },
      //This function pushes the task into the array
      addTodo() {
        this.todos.push({
          title: this.newTodo,
          done: false
        });
        this.saveHistory();
        this.newTodo = '';
      },
      //This function changes the status of the tasks
      toggle(todo)
      {
        const todoIndex = this.todos.indexOf(todo);
        this.todos[todoIndex].done = true;
        this.saveHistory();
      },
      //This function remove the todo from the array
      removeTodo(todo) {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
        this.saveHistory();
      },
      //This function sets the taks to true
      allDone() {
          this.todos.forEach(todo => {
          todo.done = true;
        });
      },
      //This function loads all the todos list 
      loadHistory() {
      if (localStorage.getItem("Data")) {
        try {
          this.Data = JSON.parse(localStorage.getItem("Data"));
        } catch (e) {
          localStorage.removeItem("Data");
        }
      }
    },
    //This function saves the todos array into the local storage
    saveHistory(){
      const parsed = JSON.stringify(this.todos);
      localStorage.setItem("Data", parsed);
    },
    //This function clears the data in the local storage
    DeleteHistory() {
      this.Data = [];
      localStorage.clear();
    },
  }
}
</script>


<style>

@import url('https://fonts.googleapis.com/css?family=Nunito&display=swap');

*{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

body
{
    background-color: #0093E9;
}

.header
{
    color: white;
    font-family: 'Nunito',sans-serif;
    text-align: center;
    margin: 25px;
}
.container
{
   position: absolute;
   top: 50%;
   left: 50%;
   transform: translate(-50%,-50%);
   height: 500px;
   width: 550px;
}
.add-element input{
    padding: 6px 4px;
    width: 70%;
}
.add-element button
{
    margin: 5px;
    padding: 7px 14px;
    background-color: #00b0ff;
    border: none;
    border-radius: 4px;
    color: #fafafa;
}
.add-element
{
    align-content: center;
    width: 100%;
    height: 20%;
}
.element-input
{
    background-color: #fafafa;
    width: 100%;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 2px 8px rgba(0,0,0,.16); 
}
.element-list
{
    font-family: 'Nunito',sans-serif;
    margin-top: 3px;
    overflow-y: scroll;
    width: 100%;
    height: 80%;
    overflow-x: hidden;
}
.element-list input{
  padding: 6px 4px;
  width: 60%;
}
.element-list button{
    margin: 5px;
    padding: 7px 14px;
    background-color: #00b0ff;
    border: none;
    border-radius: 4px;
    color: #fafafa;
}
li{
    font-family: 'Nunito',sans-serif;;
    list-style: none;
    padding: 10px 5px;
    background-color: #fff;
    width: 98%;
    margin-left: 1%;
    margin-top: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,.16);
    border-radius: 5px;
    transition: .4s;
}
button:hover{
    background-color: #1565c0;
}

li:hover{
    background-color: #e0e0e0;
}
.done
{
    text-decoration: line-through;
    background-color: #e0e0e0;
}
@media screen and (max-width: 500px){
  .contanier{
    width: 500px;
  }
  .add-element{
    margin: 70px;
    width: 100%;
    height: 10%;
  }
  .add-element input{
   width: 55%;
  } 
  .element-input{
    width: 80%;
  }
  .element-list{
    width: 95%;
    height: 60%;
  }
  .element-list li{
    width: 85%;
    height: 60%;
    margin-left: 14%;
  }
   
}
</style>
