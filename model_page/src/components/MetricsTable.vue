<template>
  <div class="card">
    <h2 style="margin:0 0 8px 0">{{ dataset.name }} metrics for {{ entry.model }}</h2>

    <table v-if="dataset.task === 'binary'">
      <tbody>
        <tr v-for="(v, k) in binaryRows" :key="k">
          <th>{{ k }}</th><td>{{ fmt(v) }}</td>
        </tr>
      </tbody>
    </table>

    <div v-else>
      <table>
        <tbody>
          <tr><th>accuracy</th><td>{{ fmt(entry.metrics?.accuracy) }}</td></tr>
          <tr><th>macro_precision</th><td>{{ fmt(entry.metrics?.macro_precision) }}</td></tr>
          <tr><th>macro_recall</th><td>{{ fmt(entry.metrics?.macro_recall) }}</td></tr>
          <tr><th>macro_f1</th><td>{{ fmt(entry.metrics?.macro_f1) }}</td></tr>
        </tbody>
      </table>

      <details v-if="entry.metrics?.per_class" style="margin-top:10px;">
        <summary>per-class details</summary>
        <table>
          <thead><tr><th>class</th><th>precision</th><th>recall</th><th>f1</th></tr></thead>
          <tbody>
            <tr v-for="(vals, cname) in entry.metrics.per_class" :key="cname">
              <td>{{ cname }}</td>
              <td>{{ fmt(vals.precision) }}</td>
              <td>{{ fmt(vals.recall) }}</td>
              <td>{{ fmt(vals.f1) }}</td>
            </tr>
          </tbody>
        </table>
      </details>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  dataset: { type: Object, required: true },
  entry: { type: Object, required: true }
})

const binaryRows = computed(() => {
  return {
    accuracy: props.entry.metrics?.accuracy,
    precision: props.entry.metrics?.precision,
    recall: props.entry.metrics?.recall,
    f1: props.entry.metrics?.f1,
    auc: props.entry.metrics?.auc,
  }
})

const fmt = v => {
  if (v === null || v === undefined) return '-'
  if (typeof v === 'number') {
    if (v <= 1 && v >= 0) return v.toFixed(3)
    return String(Math.round(v * 1000) / 1000)
  }
  return String(v)
}
</script>

<style scoped>
.card{ border: 1px solid #e5e7eb; border-radius: 12px; padding: 16px; box-shadow: 0 1px 2px rgba(0,0,0,.04); }
table{ width:100%; border-collapse: collapse; }
th, td{ text-align:left; padding: 6px 8px; border-bottom: 1px solid #eee; }
th{ width: 180px; font-weight:600; }
</style>
