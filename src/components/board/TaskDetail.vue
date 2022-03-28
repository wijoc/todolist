<template>
    <div class="card shadow-lg">
        <div class="card-header">
            <div class="row">
                <div class="col-9">
                    <h5 class="card-title">{{ task.title }}</h5>
                    <p class="text-muted small">{{ task.due_date }}</p>
                </div>
                <div class="col-3 justify-content-end">
                    <button class="btn btn-sm btn-danger text-white fw-bold" @click="deleteTask(task.id)">
                        <i class="bi bi-trash2-fill"></i> Delete
                    </button>
                </div>
            </div>
        </div>
        <div class="card-body">
            <div class="list-group">
                <!-- Description -->
                <div class="accordion" id="accordionExample">
                    <div class="accordion-item">
                        <h2 class="accordion-header" id="panelsStayOpen-headingOne">
                            <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#panelsStayOpen-collapseOne" aria-expanded="true" aria-controls="panelsStayOpen-collapseOne">
                                Description
                            </button>
                        </h2>
                        <div id="panelsStayOpen-collapseOne" class="accordion-collapse collapse show" aria-labelledby="panelsStayOpen-headingOne">
                            <div class="accordion-body alert-info">
                                {{ task.description }}
                            </div>
                        </div>
                    </div>
                    <div class="accordion-item">
                        <h2 class="accordion-header" id="panelsStayOpen-headingTwo">
                            <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#panelsStayOpen-collapseTwo" aria-expanded="false" aria-controls="panelsStayOpen-collapseTwo">
                                Detail Task
                            </button>
                        </h2>
                        <div id="panelsStayOpen-collapseTwo" class="accordion-collapse collapse" aria-labelledby="panelsStayOpen-headingTwo">
                            <div class="accordion-body alert-secondary" v-if="task.details && task.details.length > 0">
                                <div class="form-check" v-for="(td, i) in task.details" :key="i">
                                    <input class="form-check-input" type="checkbox" value="" id="detailTask">
                                    <label class="form-check-label" for="detailTask">
                                        {{ task.details[i].title }}
                                    </label>
                                </div>
                            </div>
                            <div class="accordion-body alert-danger" v-else>
                                No detail task available!
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'TasksList',
  props: {
    task: Object
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
    }
  }
}
</script>
