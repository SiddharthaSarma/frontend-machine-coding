<script setup lang="ts">
  import SelectableGrid from './components/SelectableGrid.vue';
</script>

<template>
  <div class="container">
    <h1>Selectable grid</h1>
    <SelectableGrid :rows="10" :columns="10" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
