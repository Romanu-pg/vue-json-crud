<script setup>
import { reactive } from 'vue'

defineProps({
  disabled: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['create'])

const form = reactive({
  name: '',
  color: 'green',
})

const submitForm = () => {
  const name = form.name.trim()

  if (!name) {
    return
  }

  emit('create', {
    name,
    color: form.color,
  })

  form.name = ''
  form.color = 'green'
}
</script>

<template>
  <form class="crud crud-form" @submit.prevent="submitForm">
    <div class="col-2 col-2--form">
      <h3>Add Crud</h3>

      <div class="form-control">
        <label for="crud-name">Name:</label>
        <input
          id="crud-name"
          v-model="form.name"
          type="text"
          placeholder="New crud"
          autocomplete="off"
          :disabled="disabled"
        />
      </div>

      <div class="form-control">
        <label for="crud-color">Color:</label>
        <select id="crud-color" v-model="form.color" :disabled="disabled">
          <option value="red">Red</option>
          <option value="green">Green</option>
        </select>
      </div>

      <button type="submit" :disabled="disabled || !form.name.trim()">Add</button>
    </div>
  </form>
</template>

<style scoped>
.crud-form {
  display: block;
}

.col-2--form {
  margin-left: 0;
}

.form-control {
  margin: 0.5em 0;
}

.form-control label {
  display: inline-block;
  min-width: 48px;
}

.form-control input {
  width: 185px;
}
</style>
