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

        <div class="output-reason-container">
          <div class="example-box result-box">
            <label>Model's Output:</label>
            <p class="example-output" :class="resultClass">
              {{ exampleResult }} ({{ isCorrect ? 'True' : 'False' }})
            </p>
          </div>

          <div class="example-box reason-box">
            <label>Reasoning:</label>
            <p class="example-reason">{{ exampleReason }}</p>
          </div>
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

const TRUE_LABEL = "Positive";

const exampleResult = computed(() => {
  const model = selectedModel.value;
  switch (model) {
    case "CNN":
    case "RNN":
    case "LSTM":
      return "Negative";
    case "Bi-LSTM":
    case "BERT":
    case "RoBERTa":
      return "Positive";
    default:
      return "N/A";
  }
})

const isCorrect = computed(() => {
  return exampleResult.value === TRUE_LABEL;
});

const exampleReason = computed(() => {
  const model = selectedModel.value;
  switch (model) {
    case "CNN":
      return "Lacks sequential context";
    case "RNN":
      return "Short-term memory/Recent bias";
    case "LSTM":
      return "Unidirectional context is insufficient";
    case "Bi-LSTM":
      return "Bidirectional context captured negation";
    case "BERT":
      return "Transformer self-attention understood irony";
    case "RoBERTa":
      return "Transformer self-attention understood irony";
    default:
      return "No simulation data available.";
  }
})

const resultClass = computed(() => {
  if (!exampleResult.value) return '';
  return isCorrect.value ? 'result-positive' : 'result-negative';
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

/* V-DECK: 新增的容器样式 */
.output-reason-container {
  display: grid;
  /* 自动宽度 (fit-content) + 剩余空间 (1fr) */
  grid-template-columns: auto 1fr;
  gap: 16px;
  align-items: start; /* 顶部对齐 */
}

.result-box {
  margin-top: 0; /* 移除顶部外边距 */
  margin-bottom: 0; /* 移除底部外边距 */
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

.reason-box {
  margin-top: 0; /* 移除顶部外边距 */
  margin-bottom: 0; /* 移除底部外边距 */
}
.example-reason {
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  font-size: 0.95rem;
  background-color: #f9fafb;
  padding: 12px;
  border-radius: 6px;
  border: 1px solid #d1d5db;
  margin: 0;
  line-height: 1.5;
  color: #4b5563;
  font-style: italic;
  width: 100%; /* 确保填满网格列 */
  box-sizing: border-box; /* 确保 padding 不会撑破宽度 */
}
</style>