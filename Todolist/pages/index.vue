<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-btn rounded color="primary" dark @click="handleClick">
       Add new Todo
      </v-btn>
      <v-btn rounded color="error" dark @click="clearAll">
       Delete All
      </v-btn>
        <v-alert v-if="todos.length === 0" dense color="primary" type="info">
          Aucune Todo sauvegardée
        </v-alert>
        <div v-else>
          <v-card v-for="(todo, index) in todos" :key="index">
            <v-card-title class="headline">
              Todo list
            </v-card-title>
            <v-card-actions>
              <v-btn rounded color="error" dark>
                Delete
              </v-btn>
              {{todo.date}}
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
        clearOne() {
            
        },
        handleClick() {
            this.todos.push({title:"", description:"", date:new Date().toLocaleString()});
        }
    }
}
</script>