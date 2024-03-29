<script setup lang="ts">
  import { ref, computed } from 'vue';

  const props = defineProps<{
    rows: number;
    columns: number;
  }>();

  const gridState = computed(() =>
    new Array(props.rows * props.columns).fill(false),
  );
  const columns = computed(() => props.columns);
  const boxWidth = ref('50px');

  const isMouseDown = ref(false);
  const selectedList = ref<number[]>([]);
  const handleMouseDown = (val: number) => {
    isMouseDown.value = true;
    selectedList.value = [val];
  };

  const handleMouseEnter = (val: number) => {
    if (isMouseDown.value) {
      const start = selectedList.value[0];
      const end = val;
      const startRow = Math.floor((start - 1) / props.columns);
      const endRow = Math.floor((end - 1) / props.columns);
      const startCol = (start - 1) % props.columns;
      const endCol = (end - 1) % props.columns;
      for (
        let i = Math.min(startRow, endRow);
        i <= Math.max(startRow, endRow);
        i++
      ) {
        for (
          let j = Math.min(startCol, endCol);
          j <= Math.max(startCol, endCol);
          j++
        ) {
          selectedList.value.push(i * props.columns + j + 1);
        }
      }
    }
  };

  const handleMouseUp = () => {
    isMouseDown.value = false;
  };
</script>

<template>
  <div class="grid" @mouseup="handleMouseUp">
    <div
      class="box"
      v-for="(_, index) in gridState"
      :class="{ selected: selectedList.includes(index + 1) }"
      :key="index"
      @mousedown="handleMouseDown(index + 1)"
      @mouseenter="handleMouseEnter(index + 1)"
    >
      {{ index + 1 }}
    </div>
  </div>
</template>

<style scoped>
  .grid {
    display: grid;
    grid-template-columns: repeat(v-bind(columns), v-bind(boxWidth));
    user-select: none;
    gap: 0.25rem;
  }

  .box {
    border: 1px solid black;
    width: v-bind(boxWidth);
    height: v-bind(boxWidth);
    text-align: center;
    line-height: v-bind(boxWidth);
  }
  .selected {
    background-color: #ccc;
  }
</style>
