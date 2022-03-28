<template>
    <div class="card shadow-lg">
        <h5 class="card-header">List Title</h5>
        <div class="card-body">
            <div class="list-group">
                <!-- Task List -->
                <div class="list-group-item list-group-item-action" v-for="(t, i) in tasks" :key="i">
                    <div class="row">
                        <h4 class="col-12">{{ t.title }}</h4>
                        <div class="col-12 d-flex justify-content-center ms-1 row">
                            <button class="col me-2 btn btn-sm btn-success" v-if="t.status == false" @click.left="completedTask(t.id)"><i class="bi bi-check-circle-fill"></i> Done</button>
                            <button class="col me-2 btn btn-sm btn-outline-danger" v-else-if="t.status == true" @click.left="unfinishedTask(t.id)"><i class="bi bi-check-circle-fill"></i> Unfinish</button>
                            <button class="col me-2 btn btn-sm btn-warning"><i class="bi bi-pencil-fill"></i></button>
                            <button class="col me-2 btn btn-sm btn-info" @click.left="showTaskDetail(t.id)"><i class="bi bi-info-circle-fill"></i></button>
                            <button class="col btn btn-sm btn-danger" @click.left="deleteTask(t.id)"><i class="bi bi-trash2-fill"></i></button>
                        </div>
                    </div>
                </div>
                <!-- End Task List -->
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'TasksList',
  props: {
    tasks: Array
  },
  methods: {
    showTaskDetail (taskID) {
      this.$emit('detail', taskID)
    },
    deleteTask (taskID) {
      var confirmDelete = confirm('Hapus ' + taskID + '?')
      if (confirmDelete === true) {
        this.$emit('delete', taskID)
      } else {
        alert('Canceled!')
      }
    },
    completedTask (taskID) {
      this.$emit('done', taskID)
    }
  }
}
</script>
