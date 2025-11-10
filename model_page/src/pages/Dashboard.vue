<template>
  <div class="page">
    <header class="topbar">
      <h1>Model Metrics Dashboard</h1>
    </header>

    <section class="controls">
      <Selectors
        :models="models"
        :datasets="datasets"
        v-model:model="selectedModel"
        v-model:dataset="selectedDataset"
      />
    </section>

    <section v-if="currentEntry" class="content">
      <MetricsTable :dataset="selectedDataset" :entry="currentEntry" />
      <Charts :figures="currentEntry.figures" />
    </section>

    <section v-else class="empty">
      <p>No data for this combination. Pick a model and dataset that exist in metrics.json.</p>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import Selectors from '../components/Selectors.vue'
import MetricsTable from '../components/MetricsTable.vue'
import Charts from '../components/Charts.vue'

const models = ref([])
const datasets = ref([])
const selectedModel = ref('')
const selectedDataset = ref(null)
const raw = ref(null)

onMounted(async () => {
  const res = await fetch('/src/data/metrics.json')
  raw.value = await res.json()
  models.value = raw.value.models || []
  datasets.value = raw.value.datasets || []

  selectedModel.value = models.value[0] || ''
  selectedDataset.value = datasets.value[0] || null
})

const currentEntry = computed(() => {
  if (!raw.value || !selectedDataset.value || !selectedModel.value) return null
  const ds = selectedDataset.value
  const found = ds.entries?.find(e => e.model === selectedModel.value)
  return found || null
})
</script>

<style scoped>
.page{ max-width: 1100px; margin: 0 auto; padding: 24px; }
.topbar{ display:flex; align-items:center; justify-content:space-between; margin-bottom: 16px; }
.controls{ margin: 12px 0 24px; }
.content{ display:grid; grid-template-columns: 1fr; gap: 16px; }
@media (min-width: 900px){
  .content{ grid-template-columns: 1fr 1fr; }
}
.empty{ color:#6b7280; padding: 16px; }
</style>
