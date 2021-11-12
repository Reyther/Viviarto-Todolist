<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-btn rounded color="primary" dark @click="handleClick" class="mb-4">
       Add new Todo
      </v-btn>
      <v-btn rounded color="error" dark @click="clearAll" class="mb-4 ml-3">
       Delete All
      </v-btn>
        <v-alert v-if="todos.length === 0" dense color="primary" type="info">
          Aucune Todo sauvegardée
        </v-alert>
        <div v-else>
          <v-card v-for="(todo, index) in todos" :key="index" class="mb-4">
            <v-card-text>
              <v-text-field v-model="todo.title" label="Title" :rules="rules"
              ></v-text-field>
              <v-textarea v-model="todo.description" label="Description" :rules="rules"
              ></v-textarea>
            </v-card-text>
            <v-card-actions class="justify-end pb-3">
              {{todo.date}}
              <v-btn rounded color="error" dark @click="clearOne(index)" class="ml-3">
                Delete
              </v-btn>
            </v-card-actions>
          </v-card>
        </div>
    </v-col>
  </v-row>
</template>

<script>

const STORAGE_KEY = 'vue-todolist';
let todoStorage = {
    fetch() {
        let todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
        if (todos.length === 0) {
            return [];
        }
        todos.forEach((todo, index) => {
            todo.id = index;
        })
        return todos;
    },
    save(todos) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
    },
    clear() {
        localStorage.removeItem(STORAGE_KEY);
    }
};

export default {
    name: 'App',
    data() {
        return {
            todos: todoStorage.fetch(),
            rules: [
              value => !!value || 'Required.',
            ],
        }
    },
    watch: {
      todos: {
        handler(todos) {
          todoStorage.save(todos);
        },
        deep: true,
      }
    },
    methods: {
        clearAll() {
            this.todos = [];
        },
        clearOne(index) {
            this.todos.splice(index,1);
        },
        handleClick() {
            this.todos.push({title:"", description:"", date:new Date().toLocaleString()});
        }
    }
}
</script>