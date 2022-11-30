I spent four days on the test. I was able to add edit functionality on the todos when a user double clicks on a particular todo.

I spent a lot of time working on the Mode. Finding effective measure on implementing dark and light mode was quite challenging.

Multiple v-models: Vue 3 allows us to pass multiple v-models to our components by specifying their names for two-way binding. below a i was able to use several v-models to display the todos


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


I would track down by running the tests that simulate production-level traffic on my development-stage applications. I had tracked down while creating Python/Django web application.

The test was fun and above all helpfull, i was able to learn alot through the test. I didn't face any difficulty on solving the test. I would suggest the you could be more specific on the duration to be spent on the test.
