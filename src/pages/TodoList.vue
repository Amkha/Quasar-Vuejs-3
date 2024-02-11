<template>
  <div class="row q-pa-sm bg-primary">
    <q-input
      class="col"
      bg-color="white"
      filled
      v-model="newTask"
      placeholder="Add task"
      dense
      @keyup.enter="addTask(newTask)"
    >
      <template v-slot:append>
        <q-btn @click="addTask(newTask)" round dense flat icon="add" />
      </template>
    </q-input>
  </div>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white" separator bordered>
      <!-- @click="toggleTodoDone(todo)"  works also -->
      <q-item
        q-ripple
        v-for="(todo, index) in todos"
        :key="todo.id"
        @click="todo.done = !todo.done"
        clickable
        :class="{ 'done bg-blue-1' : todo.done }"
      >
        <q-item-section avatar>
          <q-checkbox v-model="todo.done" color="primary" class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ todo.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="todo.done" side>
          <q-btn round dense flat color="primary" icon="delete" @click.stop="deleteTodo(index)"></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!todos.length">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-5 text-primary text-center">No Task</div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';
const $q = useQuasar();

interface Todo {
  id: number;
  title: string;
  done: boolean;
}
let newTask = ref("");

const todos = ref<Todo[]>([
  // {
  //   id: 1,
  //   title: 'Write code',
  //   done: true
  // },
  // {
  //   id: 2,
  //   title: 'Test code',
  //   done: false
  // },
  // {
  //   id: 3,
  //   title: 'Compile code',
  //   done: false
  // },
]);
function toggleTodoDone(todo: Todo) {
  console.log(todo);
  todo.done = !todo.done;
}
function addTask(task: string) {
  console.log(task);
  const newTodo: Todo = {
    id: Math.random(),
    title: task,
    done: false
  };
  todos.value.push(newTodo);
  newTask.value = "";
}
function deleteTodo(index: number) {
  $q.dialog({
    title: 'Confirm',
    message: 'Would you like to turn on the wifi?',
    cancel: true,
    persistent: true
  }).onOk(() => {
    // console.log('>>>> OK')
    todos.value.splice(index, 1);
    $q.notify({
      message: 'Todo is deleted...',
      color: 'purple'
    })
  }).onCancel(() => {
    // console.log('>>>> Cancel')
  }).onDismiss(() => {
    // console.log('I am triggered on both OK and Cancel')
  })


}
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
