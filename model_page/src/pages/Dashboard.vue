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

      <div class="prediction-demo">
        <h3>Single Prediction (Simulated)</h3>
        <div class="example-box">
          <label>Example Input:</label>
          <p class="example-text-display">{{ exampleSentence }}</p>
        </div>

        <div class="example-box result-box">
          <label>Model's Output:</label>
          <p class="example-output" :class="resultClass">{{ exampleResult }}</p>
        </div>
      </div>
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

const exampleSentence = ref(
  "The actors' performances were amazing, the visuals stunning, and the plot deeply moving. I absolutely would not recommend this film to anyone who enjoys being bored, wasting time, or empty plots."
)

const exampleResult = computed(() => {
  const model = selectedModel.value;

  switch (model) {
    case "CNN":
      return "Negative - Lacks sequential context";
    case "RNN":
      return "Negative - Short-term memory/Recent bias";
    case "LSTM":
      return "Negative - Unidirectional context is insufficient";
    case "Bi-LSTM":
      return "Positive - Bidirectional context captured negation";
    case "BERT":
      return "Positive - Transformer self-attention understood irony";
    case "RoBERTa":
      return "Positive - Transformer self-attention understood irony";
    default:
      return "N/A";
  }
})

const resultClass = computed(() => {
  if (!exampleResult.value) return '';
  if (exampleResult.value.startsWith('Positive')) {
    return 'result-positive';
  } else if (exampleResult.value.startsWith('Negative')) {
    return 'result-negative';
  }
  return '';
})

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
  .content{
    grid-template-columns: 1fr 1fr;
    grid-template-areas:
      "table charts"
      "demo  demo";
  }
  .content > :nth-child(1) { grid-area: table; }
  .content > :nth-child(2) { grid-area: charts; }
  .content > :nth-child(3) { grid-area: demo; }
}

.empty{ color:#6b7280; padding: 16px; }

.prediction-demo {
  border-top: 1px solid #e5e7eb;
  padding-top: 24px;
  margin-top: 16px;
}
.prediction-demo h3 {
  margin: 0;
  padding: 0;
  margin-bottom: 24px;
}
.example-box {
  margin-bottom: 16px;
}
.example-box label {
  font-weight: 600;
  color: #4b5563;
  display: block;
  margin-bottom: 4px;
}
.example-text-display {
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  font-size: 1rem;
  background-color: #f9fafb;
  padding: 12px;
  border-radius: 6px;
  border: 1px solid #d1d5db;
  margin: 0;
  line-height: 1.5;
}
.result-box {
  margin-top: 16px;
}
.example-output {
  font-weight: 700;
  font-size: 1.1rem;
  padding: 12px;
  border-radius: 6px;
  border: 1px solid;
  width: fit-content;
  margin: 0;
}
.result-positive {
  color: #059669;
  background-color: #ecfdf5;
  border-color: #a7f3d0;
}
.result-negative {
  color: #dc2626;
  background-color: #fee2e2;
  border-color: #fca5a5;
}
</style>