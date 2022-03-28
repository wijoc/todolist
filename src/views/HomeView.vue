<template>
  <div class="board p-3">
    <div class="row">
      <!-- Card Form -->
      <div class="col-lg-8 col-md-12 mb-3">
        <div class="card shadow-lg">
          <h5 class="card-header">Form New Task</h5>
          <form @submit.prevent="submitTask()">
            <div class="card-body">
              <div class="mb-3 pb-2 border-bottom" v-if="infoVisible === 'list'">
                <Form @updateValue="updateTaskData($event)" :inputs="inputs" line="outofline" purpose="Task"/>
              </div>

              <div class="row mb-2">
                <h5 class="col">Detail Form</h5>
                <span class="btn btn-outline-success col-md-2" @click.left="addDetail(1)">
                  <i class="bi bi-plus-circle-fill"></i>
                  Add detail
                </span>
              </div>
              <div class="row border border-bottom mb-1" v-for="i in detailAmount" :key="i">
                <div class="col-md-11 col-sm-12 ">
                  <Form @updateValue="updateTaskDetail(i - 1, $event)" :inputs.sync="inputs.details[i - 1]" line="inline" purpose="Detail"/>
                </div>
                <div class="col-md-1 col-sm-12 text-center align-middle pt-3">
                  <span class="text-danger fs-3" @click.left="removeDetail(i - 1, 1)">
                    <i class="bi bi-dash-circle"></i>
                  </span>
                </div>
              </div>
            </div>
            <div class="card-footer">
              <button type="submit">submit</button>
            </div>
          </form>
        </div>
      </div>
      <!-- End card form -->

      <!-- Card Tasks -->
      <div class="col-lg-4 col-md-12 mb-3">
        <TasksList v-if="infoVisible === 'list'" :infoVisible.sync="infoVisible" :tasks="tasks" @detail="showTaskDetail($event)" @delete="deleteTask($event)" @done="completedTask($event)"/>
        <TaskDetail v-else-if="infoVisible === 'detail'" :infoVisible.sync="infoVisible" :task="task" @close="showList()" @delete="deleteTask($event)"/>
      </div>
      <!-- End Card Tasks -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BoardLayout from '@/layouts/BoardLayout.vue'

import Form from '@/components/board/TheForm.vue'
import TasksList from '@/components/board/TasksList.vue'
import TaskDetail from '@/components/board/TaskDetail.vue'

export default {
  name: 'HomeView',
  components: {
    Form,
    TasksList,
    TaskDetail
  },
  data () {
    return {
      infoVisible: 'list',
      tasks: [],
      task: {},
      detailAmount: 0,
      inputs: {
        title: '',
        due_date: '',
        description: '',
        status: false,
        details: [{
          title: '',
          due_date: '',
          description: '',
          status: false
        }]
      },
      input: {
        title: '',
        due_date: '',
        description: '',
        status: false
      }
    }
  },
  methods: {
    emptyInputVal () {
      /** Empty form */
      this.input.title = ''
      this.input.due_date = ''
      this.input.description = ''
    },
    updateTaskData (newValue) { // Update tasks data
      this.inputs.title = newValue.title
      this.inputs.due_date = newValue.due_date
      this.inputs.description = newValue.description

      this.emptyInputVal()
    },
    updateTaskDetail (index, newValue) { // Update tasks data
      this.inputs.details[index] = {
        title: newValue.title,
        due_date: newValue.due_date,
        description: newValue.description,
        status: false
      }
      this.inputs.details[index + 1] = {
        title: '',
        due_date: '',
        description: '',
        status: false
      }
      // this.inputs.details.push({
      //   title: newValue.title,
      //   due_date: newValue.due_date,
      //   description: newValue.description,
      //   status: false
      // })
      // this.emptyInputVal()

      console.log(newValue.title)
      // console.log(index)
      console.log(this.inputs.details)
    },
    addDetail (value) {
      this.detailAmount += value
    },
    removeDetail (index, value) {
      this.inputs.details.splice(index, value)
      this.detailAmount -= value
    },
    getTasksList () {
      const self = this
      axios.get('http://localhost:8000/api/todo', {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('access_token'),
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          if (response.data.code === 200) {
            this.tasks = response.data.data
          }
        })
        .catch(function (error) {
          console.log(error.response)
          if (error.response.status === 400) {
            if (error.response.data.error === 'Provided token is expired.') {
              alert('You need to login!')
              self.$router.push('/login')
            }
          }
        })
    },
    submitTask () {
      function findEmpty (x) {
        return x.title === ''
      }

      var indexToRemove = this.inputs.details.findIndex(findEmpty)
      if (indexToRemove > -1) {
        this.inputs.details.splice(indexToRemove, 1)
        this.submitTask()
      } else {
        axios.post('http://localhost:8000/api/todo', this.inputs, {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('access_token'),
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            var resp = response.data
            if (resp.code === 200) {
              /** Empty form */
              this.inputs.title = ''
              this.inputs.due_date = ''
              this.inputs.description = ''
              this.inputs.status = false
              this.inputs.details = [this.input]
              this.detailAmount = 0

              this.getTasksList()
              alert(resp.message)
            }
          })
          .catch(function (error) {
            var resp = error.response
            if (resp.status === 422) {
              if (resp.data.title) {
                alert('Error : ' + resp.data.title)
              }
              if (resp.data.due_date) {
                alert('Error : ' + resp.data.due_date)
              }
            } else {
              alert('Error : ' + resp.data.message)
            }
          })
      }
    },
    deleteTask (taskID) {
      axios.delete('http://localhost:8000/api/todo/' + taskID, {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('access_token'),
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          this.datas = response.data
          console.log(response.data)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    showTaskDetail (taskID) {
      this.infoVisible = 'detail'
      axios.get('http://localhost:8000/api/todo/' + taskID, {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('access_token'),
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          if (response.data.code === 200) {
            this.task = response.data.data
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    updateTask (taskID, updateData) {
      var newUpdateData = updateData
      newUpdateData.name = updateData.title
      newUpdateData.status = !updateData.status

      axios.put('http://localhost:8000/api/todo/' + taskID, newUpdateData, {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('access_token'),
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          alert('Tasks with id : ' + taskID + ' updated!')
          this.showList()
        })
        .catch(function (error) {
          var resp = error.response
          if (resp.status === 422) {
            if (resp.data.name) {
              alert('Error : ' + resp.data.name)
            }
          } else {
            alert('Error : ' + resp.data.message)
          }
        })
    },
    completedTask (taskID) {
      axios.get('http://localhost:8000/api/todo/' + taskID, {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('access_token'),
          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          this.task = response.data.data
          console.log(response.data.data)
          // this.updateTask(taskID, this.task)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    showList () {
      this.infoVisible = 'list'
    }
  },
  beforeCreate () {
    if (!localStorage.getItem('access_token')) {
      this.$router.push('/login')
    }
  },
  created () {
    this.$emit('update:layout', BoardLayout) // Update layout with VisitorLayout.vue
  },
  mounted: function () {
    this.getTasksList()
  }
}
</script>
