<script setup>
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'
import CrudForm from './components/CrudForm.vue'
import CrudList from './components/CrudList.vue'

const API_URL = 'http://localhost:3000/cruds'

const cruds = ref([])
const loading = ref(false)
const saving = ref(false)
const error = ref('')

const muted = computed(() => loading.value || saving.value)

const setError = (message, err) => {
  error.value = message
  console.error(err)
}

const getCruds = async () => {
  loading.value = true
  error.value = ''

  try {
    const { data } = await axios.get(API_URL)
    cruds.value = data
  } catch (err) {
    setError('No se pudieron cargar los registros. Revisa que JSON Server este activo.', err)
  } finally {
    loading.value = false
  }
}

const createCrud = async (payload) => {
  saving.value = true
  error.value = ''

  try {
    const { data } = await axios.post(API_URL, payload)
    cruds.value = [...cruds.value, data]
  } catch (err) {
    setError('No se pudo crear el registro.', err)
  } finally {
    saving.value = false
  }
}

const updateCrud = async (id, payload) => {
  saving.value = true
  error.value = ''

  try {
    const { data } = await axios.patch(`${API_URL}/${id}`, payload)
    cruds.value = cruds.value.map((crud) => (crud.id === id ? data : crud))
  } catch (err) {
    setError('No se pudo actualizar el registro.', err)
  } finally {
    saving.value = false
  }
}

const deleteCrud = async (id) => {
  saving.value = true
  error.value = ''

  try {
    await axios.delete(`${API_URL}/${id}`)
    cruds.value = cruds.value.filter((crud) => crud.id !== id)
  } catch (err) {
    setError('No se pudo eliminar el registro.', err)
  } finally {
    saving.value = false
  }
}

onMounted(getCruds)
</script>

<template>
  <div id="mute" :class="{ on: muted }"></div>

  <div class="heading">
    <h1>Cruds</h1>
  </div>

  <p v-if="error" class="status status--error">{{ error }}</p>

  <CrudList
    :cruds="cruds"
    :loading="loading"
    :disabled="saving"
    @update="updateCrud"
    @delete="deleteCrud"
  />

  <CrudForm :disabled="saving" @create="createCrud" />
</template>
