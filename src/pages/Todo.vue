<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg- color-primary">
      <q-input
        v-model="newTodo"
        @keyup.enter="addTodo"
        class="col"
        square
        filled
        bg-color="grey"
        placeholder="Adicionar item"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTodo" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(todo, index) in todos"
        :key="todo.tittle"
        @click="todo.done = !todo.done"
        :class="{ 'done bg-green-1': todo.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="todo.done"
            class="no-pointer-events"
            color="green"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ todo.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="todo.done" side>
          <q-item-section v-if="todo.done" side>
            <div class="button-container">
              <q-btn
                @click.stop="editTodo(index)"
                flat
                round
                dense
                color="primary"
                icon="edit"
              />
              <q-btn
                @click.stop="deleteTodo(index)"
                flat
                round
                dense
                color="negative"
                icon="delete"
              />
            </div>
          </q-item-section>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!todos.length" class="no-todos absolute-center">
      <q-icon
        name="dangerous"
        size="100px"
        color="red"
        style="margin-bottom: 10px"
      />
      <div class="text-h5 text-blacc text-center">Vazio</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "TodoPage",

  data() {
    return {
      newTodo: "",
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    deleteTodo(index) {
      this.$q
        .dialog({
          title: "Confirmação",
          message: "Tens a certeza de que queres apagar?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.todos.splice(index, 1);
          localStorage.setItem("todos", JSON.stringify(this.todos));
          this.$q.notify("Apagado com sucesso!");
        });
    },
    editTodo(index) {
      const newTitle = this.$q
        .dialog({
          title: "Edição",
          prompt: {
            model: this.todos[index].title,
            type: "text",
          },
          cancel: true,
          persistent: true,
        })
        .onOk((data) => {
          this.todos[index].title = data;
          localStorage.setItem("todos", JSON.stringify(this.todos));
          this.$q.notify("Editado com sucesso!");
        });
    },
    addTodo() {
      this.todos.push({
        title: this.newTodo,
        done: false,
      });
      this.newTodo = "";
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
});
</script>

<style lang="scss">
.done {
  .q-item_label {
    text-decoration: line-through;
    color: rgba(109, 138, 7, 0.671);
  }

  .no-todo {
    opacity: 0.5;
  }
}
</style>
