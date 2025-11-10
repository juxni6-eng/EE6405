<template>
  <div class="card">
    <div class="row">
      <label>
        Model
        <select v-model="localModel">
          <option v-for="m in models" :key="m" :value="m">{{ m }}</option>
        </select>
      </label>

      <label>
        Dataset
        <select v-model="datasetIndex">
          <option v-for="(d, idx) in datasets" :key="d.name" :value="idx">
            {{ d.name }} ({{ d.task }})
          </option>
        </select>
      </label>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  models: { type: Array, required: true },
  datasets: { type: Array, required: true },
  model: { type: String, default: '' },
  dataset: { type: Object, default: null }
})

const emit = defineEmits(['update:model', 'update:dataset'])

const localModel = computed({
  get(){ return props.model },
  set(v){ emit('update:model', v) }
})

const datasetIndex = computed({
  get(){
    if(!props.dataset) return 0
    return props.datasets.findIndex(d => d.name === props.dataset.name)
  },
  set(i){
    emit('update:dataset', props.datasets[i])
  }
})
</script>

<style scoped>
.card{ border: 1px solid #e5e7eb; border-radius: 12px; padding: 16px; box-shadow: 0 1px 2px rgba(0,0,0,.04); }
.row{ display:flex; gap: 12px; align-items:center; flex-wrap: wrap; }
label{ display:flex; gap:8px; align-items:center; font-weight:600; }
select{ padding: 6px 8px; border-radius: 8px; border:1px solid #d1d5db; }
</style>
