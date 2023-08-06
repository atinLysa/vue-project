<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <div class="greetings grid place-items-center">
    <div class="border border-white-500" style="margin-bottom: 40px">
      <img src="/src/assets/logo-9fb5691f.jpg" alt="logo" />
    </div>
    <div>
      <label for="" class="text-white text-3xl font-bold flex items-center"
        >TODO</label
      >
      <!-- input for adding a new todo -->
      <input
        type="text"
        placeholder="Enter todo here"
        class="text-xl h-[40px] w-[330px] p-5 flex-1 px-4 py-2 m-2"
        v-model="newTodo"
      />
      <!-- button for adding the todo -->
      <button
        class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 w-[158px] px-10 border border-gray-400 rounded shadow flex-1 text-center py-2 m-2"
        @click="addTodo"
      >
        Add ToDo
      </button>

      <!-- the loop -->
      <div
        class="p-3 text-white text-lg border border-white rounded-md max-h-[350px] overflow-auto"
      >
        <div v-if="todos.length === 0" class="text-white text-center">
          Nothing to display.
        </div>
        <!-- for loop for the todo list -->

        <div
          v-for="(todo, index) in todos"
          :key="todo.id"
          class="p-2 border-b-2 todo-item text-white flex flex-col items-start"
        >
          <div class="flex flex-row w-full">
            <div class="flex-grow text-sm pb-5 font-bold">
              TODO#{{ todo.id }}
            </div>
            <!-- div for the x icon, removing the todo on the list -->
            <div
              class="remove-item cursor-pointer font-semibold"
              @click="removeTodo(index)"
            >
              &times;
            </div>
          </div>

          <!-- edit part of the todo -->
          <div class="todo-item-left">
            <!-- the v-if and v-else condition hides the input field  for editing, so when i clicked the label that's the time that the hidden input field will show, its also because of the @click and editTodo method-->
            <div
              v-if="!todo.editing"
              @click="editTodo(todo)"
              class="todo-item-label mb-2 cursor-pointer"
            >
              {{ todo.title }}
            </div>

            <!-- input field for editing the todo, this is hidden initially and will only appear when the todo label is clicked -->
            <input
              ref="editTodoInput"
              v-else
              class="todo-item-edit text-black"
              type="text"
              v-model="todo.title"
              @keyup.enter="validateInput(todo)"
            />

            <!-- Show the message only when editing -->
            <div v-if="todo.editing" class="italic text-xs mt-2">
              Press enter to update
            </div>
          </div>
          <!-- edit part of the todo -->

          <!-- time stamp -->
          <div class="text-xs font-semibold place self-end">
            {{ changeTimeFormat(todo.timestamp) }}
          </div>
        </div>
      </div>
      <!-- end of the loop -->
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: "1",
      // initialize the array to empty so that the message "Nothing to display." will appear
      todos: [],
    };
  },
  methods: {
    addTodo() {
      // this line of code is for validation that an empty input wouldn't be saved
      if (this.newTodo.trim().length == 0) {
        return alert("Please enter a todo!");
      }

      // this line of code is for adding a to do item on the list
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        timestamp: new Date().getTime(), //save the current timestamp
        // editing:'false'
      });

      // this line of code adds up the todo items i keep on adding
      this.newTodo = "";
      this.idForTodo++;
    },

    // for edting to do, this line of code allows you to click the todo that you're going to edit
    editTodo(todo) {
      todo.editing = "true";
    },

    validateInput(todo) {
      if (!todo.title.trim()) {
        alert("Input cannot be empty!");
          this.$refs.todoInput.focus(); // Focus on the input field after displaying the alert
      } else {
        // Perform your desired action when the input is not empty
        this.updateTodo(todo);
      }
    },

    // for updating/saving the todo, this functions when you press enter right after you update or changed something
    updateTodo(todo) {
      todo.editing = false;
    },

    // removing the to do item on the list
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    //  timestamp
    changeTimeFormat(timestamp) {
      let date = new Date(timestamp);
      let month = date.getMonth()+1; // Get the abbreviated month name
      let day = date.getDate(); // Get the day of the month
      let year = date.getFullYear(); // Get the full year
      let hours = date.getHours();
      let minutes = date.getMinutes();
      let seconds = date.getSeconds();

      //check what to use AM or PM
      let newFormat = hours >= 12 ? "PM" : "AM";

      //find current hour in AM or PM format, i used modulo operator to find the remainder and that will be the current hour
      hours = hours % 12;

      //display "0" as "12"
      hours = hours ? hours : 12;
      minutes = minutes < 10 ? "0" + minutes : minutes;
      seconds = seconds < 10 ? "0" + seconds : seconds;

      const formattedTime =
        month +
        "/" +
        day +
        "/" +
        year +
        ", " +
        hours +
        ":" +
        minutes +
        ":" +
        seconds +
        " " +
        newFormat;

      return formattedTime;
    },
  },
};
</script>

<style scoped>
@media (min-width: 1024px) {
  .greetings h1 {
    text-align: left;
  }
}

img {
  height: 141px;
  width: 500px;
}
</style>
