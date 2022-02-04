<template>
  <div class="container mt-2">
    <template v-if="!isTaskEmpty">
      <div v-for="(task) in tasks" :key="task.id">
        <b-card :title="task.subject" class="mb-2">
          <b-card-text>{{ task.description }}</b-card-text>
           <b-button
            variant="outline-secondary"
            class="mr-2"
            @click="updateStatus(task.id, status.FINISHED)"
          >
            Concluir
          </b-button>
          <b-button
            variant="outline-secondary"
            class="mr-2"
            @click="updateStatus(task.id, status.ARCHIVED)"
          >
            Arquivar
          </b-button>
          <b-button
            variant="outline-secondary"
            class="mr-2"
            @click="edit(task.id)"
          >
            Editar
          </b-button>
          <b-button
            variant="outline-danger"
            @click="remove(task.id)"
          >
            Excluir
          </b-button>
        </b-card>
      </div>
    </template>
    <template v-else>
      <div class="empty-data mt-2">
        <img src="../assets/images/empty-data.svg" class="empty-data-image">
        <b-button
          variant="outline-primary mt-2"
          size="lg"
          to="/form"
        >
          Criar tarefas
        </b-button>
      </div>
    </template>

    <b-modal
      ref="modalRemove"
      hide-footer
      title="Exclusão de tarefa"
    >
      <div class="d-block text-center">
        Deseja realmente excluir essa tarefa? <b>{{ taskSelected.subject }}</b>
      </div>
      <div class="mt-3 d-flex justify-content-end">
        <b-button
          variant="outline-secondary"
          class="mr-2"
          @click="hideModal"
        >
          Cancelar
        </b-button>
        <b-button
          variant="outline-danger"
          @click="confirmRemoveTask(task, index)"
        >
          Excluir
        </b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
import ToastMixin from '@/mixins/toastMixins.js'
import TasksModel from '@/models/TasksModel'
import Status from '@/valueObjects/status'

export default {
  name: 'List',
  mixins: [ToastMixin],
  data () {
    return {
      tasks: [],
      taskSelected: [],
      status: Status
    }
  },
  async created () {
    this.tasks = await TasksModel.params({
      status: [
        this.status.OPEN,
        this.status.FINISHED
      ]
    }).get()
  },
  methods: {
    edit (taskId) {
      this.$router.push({ name: 'form', params: { taskId } })
    },
    async remove (taskId) {
      this.taskSelected = await TasksModel.find(taskId)

      this.$refs.modalRemove.show()
    },
    hideModal () {
      this.$refs.modalRemove.hide()
    },
    async confirmRemoveTask () {
      this.taskSelected.delete()
      this.tasks = await TasksModel.params({
        status: [
          this.status.OPEN,
          this.status.FINISHED
        ]
      }).get()

      this.hideModal()
      this.showToast('success', 'Sucesso', 'Tarefa removida com sucesso!')
    },
    async updateStatus (taskId, status) {
      const task = await TasksModel.find(taskId)
      task.status = status
      await task.save()

      this.tasks = await TasksModel.params({
        status: [
          this.status.OPEN,
          this.status.FINISHED
        ]
      }).get()
      this.showToast('success', 'Sucesso', 'Status da tarefa atualizado com sucesso!')
    }
  },
  computed: {
    isTaskEmpty () {
      return this.tasks.length === 0
    }
  }
}
</script>

<style scoped>
  .empty-data {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  .empty-data-image {
    width: 300px;
    height: 300px;
  }
</style>
