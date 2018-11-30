<template>
  <div class="home">
    <!-- Access properties from data() or computed like this. -->
    <h1>{{ title }}</h1>
    <input
      type="text"
      v-model="userInput"
      @keyup.enter="addToDo()"
    >
    <button class="add" @click="addToDo()">Add To Do</button>

    <p class="count">{{ count }}</p>

    <!-- this ul will only appear on the page if the condition inside v-if evaluates to true -->
    <ul v-if="todos.length > 0">

      <Todo
        v-for="(item, i) in todos"
        :key="item"
        @click="/*removeFromList(i) this will not work on custom Components */"
        :myPropName="item"
        :index="i"
        @itemWasClicked="removeFromList(i)"
      >
        <!-- Define custom names for props and bind them to javascript variables with ":" which is shorthand for the v-bind directive. this is how you pass data from parent to child -->

        <!-- @click event wont work on a custom component because the functionality shoud be contained at the Component level. emit an event to pass data from child to the parent component. See Todo.vue:line 13 for an example of how to emit an event. -->

        <!-- slot name corresponds to the optional <slot> tag in the child. pass any HTML from child to parent this way, or pass plain text as a prop. -->

        <span slot="title">{{item}}</span>
      </Todo>
    </ul>

    <!-- this h2 renders if the above v-if statement evaluates to false -->
    <h2 v-else>You're done! No items in this list</h2>

  </div>
</template>

<script>
  // @ is an alias to /src
  import Todo from "@/components/Todo.vue";

  export default {
    name: "ToDoList", // Defines the name of the Component in Vue DevTools
    components: { Todo }, // Register other .vue files for use in this Page via the name in the import statement on line 39.

    data() {
      // this is where you store any variables that are global to this Vue component.
      return {
        // All properties in here are immediately 'reactive' to changes.
        title: "To Do List",
        userInput: "",
        todos: []
      };
    },
    methods: {
    // this is where you define actions for the component.
      addToDo() {
        this.todos.push(this.userInput);
        this.userInput = "";
      },

      removeFromList(index) {
        this.todos.splice(index, 1);
      }
    },
    computed: {
      // This is similar to data() but each function must return data that has to be transformed in some way first.
      count() {
        // reference this in the template without the () at the end. e.g. {{ count }}
        if (this.todos.length === 1) {
          return `You have ${this.todos.length} item in your list`;
        }
        return `You have ${this.todos.length} items in your list`;
      }
    },

    watch: {
      // not discussed in class. this is an additional property that can be used to run some function when a value in data or computed changes. each name must correspond to the name used in the template. always passes 2 pieces of data automatically, the new value and the old value.

      todos(newValue, oldValue){
        // this will run anytime the todos array in data() changes.
        // we'll use this example by saving the todo list to the browser's localStorage. that way when you close the page or reload, the items in your list will not get erased.
        console.log('todos array changed. Running watcher function...', newValue);
        
        //localStorage can only store strings, so we'll turn the data into JSON first then save it under the name VueToDoList.

        window.localStorage.setItem('VueToDoList', JSON.stringify(newValue));

        console.log('todos array has been saved to localStorage');
      }
    },

    created() {
      // Lifecycle hook. run any setup logic here.
      // runs first. has access to the Vue instance, but not template.
      console.log('created() Lifecycle hook running now...')

      // on line 83 we saved our toDo list to the browsers localStorage. If that data exists, we'll retrieve it now and load it into the todos array after converting it from JSON into an array again.

      const localStorageData = window.localStorage.getItem('VueToDoList');

      if (localStorageData) {
        this.todos = JSON.parse(localStorageData);
      }
    },
    mounted() {
      // Lifecycle hook. run any logic that requires access to the template.
      console.log('mounted() Lifecycle hook running now...')
      // runs second, has access to template now.
      // Do API things in this lifecycle hook. see axios
    }
  };
</script>


<style lang="scss">
  /* You can write css or scss here! */

  /* ============== */
  /*   Variables    */
  /* ============== */
  $color-blue: #2c3e50;
  $color-green: #42b983;

  .home {
    width: 100%;
    height: 100%;
    max-width: 1200px;
    margin: auto;
    border: solid 16px $color-blue;
    border-top: none;
    border-bottom: none;
    padding: 2rem;

    /* nesting rules in scss is amazing and saves time */
    h1 {
      /* same as writing ".home h1 {...}" in .css files */
      color: $color-green;
    }

    input[type=text]{
      outline: none;
      border: none;
      border-bottom: solid 3px grey;
      width: 50%;
      max-width: 600px;
      min-width: 300px;
      font-size: 125%;
      transition: border-color .2s ease;
      margin-bottom: 2rem;
      font-weight: bold;
      color: $color-blue;

      &:focus{
        border-color: $color-green;
        transition: border-color .2s ease;
      }
    }

    button.add{
       background-color: lighten($color-blue, 25%);
      color: white;
      border: none;
      outline: none;
      padding: .5rem 1rem;
      margin: 0 1rem;
      border-radius: 0.5rem;
      transition: background-color 0.2s ease;
      cursor: pointer;

      &:hover {
        background-color: lighten($color-blue, 35%);
        transition: background-color 0.2s ease;
      }
    }

    p.count{
      text-align: left;
      font-weight: bold;
      padding-bottom: 1rem;
    }

    ul {
      list-style: none;
      text-align: left;
    }
  }
</style>

