<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
      v-model="newTask"
      @keyup.enter="addTask"
      class="col"
      square
      filled
      bg-color="white"
      placeholder="Add task"
      dense>
      <template v-slot:append>
        <q-btn
        @click="addTask"
        round
        dense
        flat
        icon="add" />

      </template>
      </q-input>
    </div>
  <q-list 
    class="bg-white"
    separator
    bordered>
      <q-item 
        v-for="(task, index) in tasks"
        @click="task.done = !task.done" 
        :key="task.tilte"
        :class="{ 'done bg-blue-1' : task.done }"
        clickable 
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
          v-model="task.done"          
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side> 
          <q-btn 
          flat round dense
          @click.stop="deleteTask(index)"
          color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon 
      name="check"
      size="100px"
      color="primary" />
      <div class="text-5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: '',
  data(){
    return {
      newTask: '',
      tasks: [        
        // {
        //   title: 'Task 1',
        //   done: false
        // },
        //       {
        //   title: 'Task 2',
        //   done: false
        // },
        //       {
        //   title: 'Task 3',
        //   done: false
        // }
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$q.notify('Task deleted')
        this.tasks.splice(index, 1);
      });      
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = '';
    }
  },
}
</script>
<style lang="scss" scoped>
 .done {
   .q-item__label {
     text-decoration: line-through;
     color: #ccc;
   }
 }
 .no-tasks {
   opacity: 0.5;
 }

</style>
