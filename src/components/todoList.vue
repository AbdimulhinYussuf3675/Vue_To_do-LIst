<template >
  <section class="container" :class="{ 'theme-dark': nightMode }">
    <section id="sectionA">
      <div class="header">
        <h3>TO DO</h3>
        <div class="mode">
          <i class="fas fa-sun icon-sun" v-if="nightMode" @click="nightMode = !nightMode"></i>
          <i class="fas fa-moon icon-moon" v-else @click="nightMode = !nightMode"></i>
        </div>
      </div>

      <input type="text" class="todo-input" placeholder="Create a new todo..." v-model="newTodo" @keyup.enter="addTodo">
    </section>
    <section class="sectionB">
      <div class="lists">
        <div class="back-img">
          <draggable v-model="todosDrag">
            <transition-group name="fade" enter-active-class="animated fadeInUp"
              leave-active-class="animated fadeOutDown">
              <div v-for="todo in todosFiltered" :key="todo.id" class="todo-items">
                <div class="todo-item-left">
                  <input type="checkbox" id="checkbox" v-model="todo.completed">
                  <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label"
                    :class="{ completed: todo.completed }">{{ todo.title }}</div>
                  <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
                    @keyup.enter="doneEdit(todo)">
                </div>
                <div class="delete" @click="deleteTodo(index)">
                  &times;
                </div>
              </div>
            </transition-group>
          </draggable>
        </div>


        <div class="extra-container">
          <div class="link1">{{ remaining }} items left</div>
          <div class="clear">
            <transition name="fade">
              <a id="clear" class="clear" @click="clearCompleted">Clear Completed</a>
            </transition>
          </div>
        </div>

      </div>
    </section>
    <div class="link">
      <a :class="{ active: filter == 'all' }" @click="filter = 'all'">All</a>
      <a :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</a>
      <a :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</a>
    </div>
    <footer>
      <p>Drag and drop to reoder list</p>
    </footer>
  </section>
</template>
  
<script>
import { VueDraggableNext } from 'vue-draggable-next'
export default {
  name: 'todo-list',
  components: {
    draggable: VueDraggableNext,
  },

  data() {
    return {
      newTodo: '',
      idForTodo: 1,
      filter: 'all',
      nightMode: false,
      todos: [
        {
          'id': 0,
          'title': 'Learn Data Algorithms',
          'completed': false,
          'editing': false
        },
        {
          'id': 1,
          'title': 'Go to for Prayers',
          'completed': false,
          'editing': false
        }
      ]
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })

      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo) {
      todo.editing = true
    },
    doneEdit(todo) {
      todo.editing = false;
    },

    deleteTodo(index) {
      this.todos.splice(index, 1)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  watch: {
    nightMode: function () {
      localStorage.setItem("nightMode", JSON.stringify(this.nightMode));
    }
  },
  created() {
    this.nightMode = JSON.parse(localStorage.getItem("nightMode"));
  }

}
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
@import url('https://use.fontawesome.com/releases/v5.8.1/css/all.css');

#sectionA {
  background-image: url(../assets/images/bg-desktop-light.jpg);
  height: 365px;
  background-repeat: no-repeat;
  margin: 0;
  background-size: 100% 88%;
}

.todo-input {
  width: 90%;
  padding: 20px 30px;
  border-radius: 7px;
  font-size: 24px;
  margin-left: 4.6%;
  margin-top: 25px;
}

.todo-input:focus {
  outline: 0;
}

.icon-sun {
  color: #fff;
}

.icon-moon {
  color: white;
}

.mode {
  margin-left: 80%;
  margin-top: -45px;
  font-size: 2.8rem;
  cursor: pointer;
}

h3 {
  color: #fff;
  font-size: 40px;
  margin-top: 60px;
  margin-left: 5%;
}

.sectionB {
  width: 93%;
  margin: 0;
  margin-left: 4%;
  font-size: 18px;
  margin-top: -15%;
  background-color: #fff;
  border-radius: 5px
}

.todo-items {
  margin-bottom: 12px;
  display: flex;
  font-size: 24px;
  animation-duration: 0.3s;
  justify-content: space-between;
  padding: 22px;
  border-bottom: 0.211px solid rgb(70, 66, 66);
}

.todo-item-label {
  margin-left: 17px;
}

.delete {
  cursor: pointer;
  margin-left: 14px;
  font-size: 38px;
}

#checkbox {
  width: 2em;
  height: 2rem;
  top: 0;
  accent-color: aqua;
  border-radius: 10px;
  border: 1px solid #ddd;
  cursor: pointer;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.delete:hover {
  color: black;
}

.todo-item-edit {
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
}

.todo-item-edit:focus {
  outline: none;
}

footer {
  padding: 84px;
  position: inherit;
  font-size: 23px;
  text-align: center;
}

p {
  color: rgb(153, 144, 144);
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
  padding: 10px;
}

.link {
  width: 93%;
  text-align: center;
  margin: 0;
  margin-left: 4%;
  font-size: 25px;
  margin-top: 7%;
  background-color: #fff;
  border-radius: 5px;
  padding: 10px;
}

.link a {
  padding: 0px 10px;
  word-wrap: normal;
  display: inline-block;
}

a:hover {
  color: blue;
}

a:focus {
  outline: none;
}

.container {
    &.theme-dark {
      color: #efefef;
      background-color: rgb(12, 2, 27);
      #sectionA {
        background-image: url(../assets/images/bg-desktop-dark.jpg);

        .todo-input {
          background-color: rgb(41, 34, 54);
        }

        .todo-items {
          margin-bottom: 12px;
          display: flex;
          animation-duration: 0.3s;
          justify-content: space-between;
          padding: 22px;
          border-bottom: 0.211px solid rgb(70, 66, 66);
        }

        .todo-input {
          color: #ccc;
        }

      }

      .sectionB {
        background-color: rgb(41, 34, 54);
        border: 1px solid #555;
      }

      .link {
        background-color: rgb(41, 34, 54);
      }
    }
  }


@media all and (min-width: 768px) {


  #sectionA {
    background-image: url(../assets/images/bg-desktop-light.jpg);
    height: 365px;
    background-repeat: no-repeat;
    margin: 0;
    background-size: 100% 100%;
  }

  .todo-input {
    width: 57%;
    padding: 15px 25px;
    border-radius: 7px;
    font-size: 24px;
    margin-top: 15px;
    margin-left: 24.6%;

  }

  .todo-input:focus {
    outline: 0;
  }


  .icon-sun {
    color: #fff;
  }

  .icon-moon {
    color: white;
  }

  .mode {
    margin-left: 77%;
    margin-top: -40px;
    font-size: 2.6rem;
    cursor: pointer;
  }

  .todo-items {
    margin-bottom: 12px;
    display: flex;
    animation-duration: 0.3s;
    justify-content: space-between;
    padding: 22px;
    border-bottom: 0.211px solid rgb(70, 66, 66);
  }

  .delete {
    cursor: pointer;
    margin-left: 14px;
  }

  h3 {
    color: #fff;
    font-size: 40px;
    margin-top: 80px;
    margin-left: 25%;
  }

  footer {
    padding: 217px;
    position: inherit;
    font-size: 18px;
    top: 20px;
    text-align: center;
  }

  p {
    margin-top: -15%;
    color: rgb(153, 144, 144);
  }

  .delete:hover {
    color: black;
  }

  .todo-item-left {
    display: flex;
    align-items: center;
  }

  .todo-item-label {
    padding: 10px;
    margin-left: 12px;
  }

  .todo-item-edit {
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
  }

  .todo-item-edit:focus {
    outline: none;
  }

  .completed {
    text-decoration: line-through;
    color: grey;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 14px;
    margin-bottom: 14px;
    padding: 10px;
    font-size: 20px;
  }

  .link {
    background-color: unset;
    margin-top: -4%;
    font-size: 20px;
  }

  .link a {
    padding: 0px 10px;
    word-wrap: normal;
    display: inline-block;
  }

  a:hover {
    color: blue;
  }

  a:focus {
    outline: none;
  }

  #checkbox {
    width: 2em;
    height: 2rem;
    top: 0;
    accent-color: aqua;
    border-radius: 10px;
    border: 1px solid #ddd;
    cursor: pointer;
  }

  .sectionB {
    width: 57%;
    margin: 0;
    margin-left: 25%;
    font-size: 18px;
    margin-top: -7%;
    background-color: #fff;
  }


  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }

  .container {
    &.theme-dark {
      color: #efefef;
      background-color: rgb(12, 2, 27);

      #sectionA {
        background-image: url(../assets/images/bg-desktop-dark.jpg);

        .todo-input {
          background-color: rgb(41, 34, 54);
        }

        .todo-items {
          margin-bottom: 12px;
          display: flex;
          animation-duration: 0.3s;
          justify-content: space-between;
          padding: 22px;
          border-bottom: 0.211px solid rgb(70, 66, 66);
        }

        .todo-input {
          color: #ccc;
        }

      }

      .sectionB {
        background-color: rgb(41, 34, 54);
        border: 1px solid #555;
      }

      .link {
        background-color: unset;
      }
    }
  }
}
</style>
  