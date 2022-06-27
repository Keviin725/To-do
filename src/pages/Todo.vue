<template>

  <q-page class="bg-grey-3 column">

    <div
    class="row q-pa-sm bg-primary">
      <q-input
      class="col"
      square
      filled
      bg-color="white"
      v-model="newTask"
      placeholder="Adicionar Tarefa"
      @keyup.enter="addTask"
      dense>


        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>

    <q-list separator bordered class="bg-white">
      <q-item
      v-for=" (task, index) in tasks" :key="task.title"
      @click="task.done = !task.done" clickable
      :class="{'done bg-blue-2' : task.done}" v-ripple>
        <q-item-section avatar>
          <q-checkbox class = "no-pointer-events" v-model="task.done" color="primary" />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>

        </q-item-section>
        <q-item-section v-if ="task.done" side>
          <q-btn
          @click.stop="deleteTask(index)"
          flat
          round
          dense
          color="primary"
          icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check"
      size="100px"
      color= "primary"/>


      <div class="text-h5 text-primary text-center">
        Sem Tarefas
      </div>
    </div>

  </q-page>
</template>

<script>
export default {
  data(){
    return{
      newTask: '',
      tasks: []
    }
  },
  methods: {
    deleteTask(index){
      this.$q.dialog({
        title:'Confirmar',
        message:'Deseja apagar?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$q.notify('Tarefa apagada')
        this.tasks.splice(index, 1)
      })

    },
    addTask(){
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  }
}
</script>
<style lang = "scss">
  .done{
    .q-item__label{
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks{
    opacity: 0.5;
  }
</style>
