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
            <button type="submit" name="button" @click="addTodo">Add</button>
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
    methods: {
      //This function takes the sentnece spoken by the user
      onEnd({lastSentence})
      {
          let word = lastSentence;
          this.newTodo = word;
          if(lastSentence.includes('remove') || lastSentence.includes('Remove') || lastSentence.includes('move'))
          {
                this.removeTodo();
          }
          else if(lastSentence.includes('Add') || lastSentence.includes('add'))
          {
            this.newTodo = word.slice('3');
            this.addTodo();
          }
          else if(lastSentence.includes('All Done') || lastSentence.includes('all done'))
          {
            this.allDone();
          }
          else if(lastSentence.includes('clear list') || lastSentence.includes('ClearList')){
            this.Clear();
          }
      },
      //This function pushes the task into the array
      addTodo() {
        this.todos.push({
          title: this.newTodo,
          done: false
        });
        this.newTodo = '';
      },
      //This function changes the status of the tasks
      toggle(todo)
      {
        const todoIndex = this.todos.indexOf(todo);
        this.todos[todoIndex].done = true;
      },
      //This function remove the todo from the array
      removeTodo(todo) {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
      },
      //This function sets the taks to true
      allDone() {
          this.todos.forEach(todo => {
          todo.done = true;
        });
      },
      //This function is used to clear the list
      Clear(){
        this.todos = [];
      }
    },
  };
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
    font-family: 'Nunito',sans-serif;
}

.header
{
    color: white;
    position: relative;
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
.add-element{
  width: 100%;
}
.add-element input{
    padding: 6px 4px;
    width: 70%;
}
.add-element button
{
    margin: 3px;
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
    box-shadow: 0 2px 8px rgba(0,0,0,0.35); 
}
.element-list
{
    font-family: 'Nunito',sans-serif;
    margin-top: 3px;
    overflow-x: auto;
    width: 100%;
    height: 80%;
    overflow-x: hidden;
}
.element-list input{
  border: none;
  padding: 6px 4px;
  width: 60%;
  color: black;

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
    margin-left: 70px;
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
    margin-left: 13%;
  }

@media screen and (max-width: 360px) {
  .container{
    width: 400px;
  }
  .add-element
  {
    width: 100%;
    margin-top: 5%;
    margin-left: 50%;
  }
  .add-element input{
    width: 100%;
  }
  .element-list{
    width: 100%;
    margin-top: 5%;
  }
  .element-list li{
     width: 80%;
  }
  .element-list input{
    width: 55%;
  }
  
}

@media screen and (max-width: 375px){
  .header{
    margin-left: 20%;
  }
  .container{
    width: 450px;
  }
  .add-element{
    width: 100%;
    margin-left: 20%;
  }
  .add-element input{
    width: 55%;
  }
  .add-element button{
    margin: 3px;
  }
  .element-list{
    margin-top: 10%;
    margin-left: 7%;
  }
  .element-list input{
    width: 57%;
  }
  .element-list button{
    margin: 2px;
  }
}

@media screen and (max-width: 320px){
  .add-element{
    margin-top: 10%;
    margin-left: 26%;
  }
  .element-list{
    margin-left: 12%;
  }
}

@media screen and (max-width: 414px){
  .add-element{
    margin-left: 20%;
  }
  .element-list{
    margin-left: 8%;
    margin-top: 10%;
  }

  
}

}

</style>
