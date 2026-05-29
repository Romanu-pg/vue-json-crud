<script setup>
import { computed } from 'vue'

const props = defineProps({
  crud: {
    type: Object,
    required: true,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['update', 'delete'])

const colors = ['red', 'green']

const image = computed(() => `/images/${props.crud.color}.png`)

const properCase = (value) => `${value}`.charAt(0).toUpperCase() + `${value}`.slice(1)

const updateCrud = (event) => {
  emit('update', props.crud.id, {
    color: event.target.value,
  })
}

const deleteCrud = () => {
  emit('delete', props.crud.id)
}
</script>

<template>
  <div class="crud">
    <div class="col-1">
      <img :src="image" :alt="`${crud.color} alien`" />
    </div>

    <div class="col-2">
      <h3>Name: {{ properCase(crud.name) }}</h3>

      <select :value="crud.color" :disabled="disabled" @change="updateCrud">
        <option
          v-for="color in colors"
          :key="color"
          :value="color"
          :selected="color === crud.color ? 'selected' : undefined"
        >
          {{ properCase(color) }}
        </option>
      </select>

      <button type="button" :disabled="disabled" @click="deleteCrud">Delete</button>
    </div>
  </div>
</template>
