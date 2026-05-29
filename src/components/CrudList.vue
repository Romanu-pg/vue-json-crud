<script setup>
import CrudItem from './CrudItem.vue'

defineProps({
  cruds: {
    type: Array,
    required: true,
  },
  loading: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['update', 'delete'])
</script>

<template>
  <p v-if="loading" class="status">Loading...</p>
  <p v-else-if="cruds.length === 0" class="status">No cruds yet.</p>

  <template v-else>
    <CrudItem
      v-for="crud in cruds"
      :key="crud.id"
      :crud="crud"
      :disabled="disabled"
      @update="(id, payload) => $emit('update', id, payload)"
      @delete="(id) => $emit('delete', id)"
    />
  </template>
</template>
