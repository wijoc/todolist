<template>
    <div class="row">
        <div class="col form-floating p-1 mb-3">
            <input type="text" class="form-control" id="task-title" placeholder="name@example.com" v-model="input.title">
            <label for="task-title">{{ purpose }} Title</label>
        </div>
        <div class="col form-floating p-1">
            <input type="date" class="form-control" id="task-date" placeholder="YYYY-mm-dd" v-model="input.due_date">
            <label for="task-date">{{ purpose }} Due Date</label>
        </div>
        <div v-if="line === 'outofline'" class="col-12 form-floating p-1">
            <textarea class="form-control" placeholder="Leave a comment here" id="task-decription" style="height: 100px" v-model="input.description"></textarea>
            <label for="task-decription">{{ purpose }} Description</label>
        </div>
        <div v-else-if="line === 'inline'" class="col form-floating p-1">
            <textarea class="form-control" placeholder="Leave a comment here" id="task-decription" v-model="input.description"></textarea>
            <label for="task-decription">{{ purpose }} Description</label>
        </div>
    </div>
</template>

<script>
export default {
  name: 'TodoForm',
  data () {
    return {
      input: this.inputs
    }
  },
  props: {
    line: String,
    purpose: String,
    amount: Number,
    inputs: Object
  },
  watch: {
    'input.title': function (value, oldValue) {
      this.$emit('updateValue', this.input)
    },
    'input.due_date': function (value, oldValue) {
      this.$emit('updateValue', this.input)
    },
    'input.description': function (value, oldValue) {
      this.$emit('updateValue', this.input)
    }
  }
}
</script>
