<template>
  <div class="card">
    <h2 style="margin:0 0 8px 0">Figures</h2>
    <div class="grid">
      <figure v-for="(src, key) in figures" :key="key">
        <img :src="src" :alt="key" />
        <figcaption>{{ key }}</figcaption>
      </figure>
    </div>
    <p v-if="!figures || Object.keys(figures).length === 0" style="margin:0;">
      No figures found for this run. Put image paths under <code>figures</code> in metrics.json.
    </p>
  </div>
</template>

<script setup>
defineProps({
  figures: { type: Object, default: () => ({}) }
})
</script>

<style scoped>
.card{ border: 1px solid #e5e7eb; border-radius: 12px; padding: 16px; box-shadow: 0 1px 2px rgba(0,0,0,.04); }
.grid{
  display:grid;
  grid-template-columns: 1fr;
  gap: 12px;
}
@media (min-width: 900px){
  .grid{ grid-template-columns: 1fr 1fr; }
}
figure{ margin:0; }
img{ width:100%; height:auto; display:block; border-radius: 10px; border:1px solid #e5e7eb; }
figcaption{ color:#6b7280; font-size: 14px; margin-top: 6px; }
</style>
