<template>
  <q-page class="q-pa-lg">
    <h5>Add task</h5>
    <q-input
      @keyup.enter="addTask"
      dense
      v-model="newTask"
      label="Add new task"
    >
      <template v-slot:append>
        <q-btn @click="addTask" round dense flat icon="add"></q-btn>
      </template>
    </q-input>
    <q-list class="q-pa-none" separator bordered>
      <q-item
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-green-2': task.done }"
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
      >
        <q-item-section class="row">
          <q-checkbox
            v-model="task.done"
            val="teal"
            class="no-pointer-events"
            color="primary"
          >
            <q-item-label>{{ task.title }}</q-item-label>
          </q-checkbox>
          <q-item-section v-if="task.done" side>
            <q-btn
              @click.stop="delTask(index)"
              flat
              dense
              round
              color="primary"
              icon="delete"
            ></q-btn>
          </q-item-section>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-task absolute-center" v-if="tasks.length <= 0">
      <div class="text-h4 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "gello",
          done: false,
        },
        {
          title: "gello1",
          done: false,
        },
        {
          title: "gello3",
          done: false,
        },
      ],
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
    delTask(index) {
      this.confirm = !this.confirm;
      this.$q
        .dialog({
          title: "Confirm",
          message: "Точно ударяем?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Задача удалена",
            color: "blue",
          });
        });
    },
  },
};
</script>

<style>
.done .q-item__label {
  text-decoration: line-through;
  color: gray;
}
.q-item__section {
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: space-between;
}
</style>
