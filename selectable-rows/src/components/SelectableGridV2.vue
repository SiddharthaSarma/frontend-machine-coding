<script setup lang="ts">
  import { ref, computed } from 'vue';

  const props = defineProps<{
    rows: number;
    columns: number;
  }>();

  const gridState = computed(() =>
    new Array(props.rows)
      .fill(false)
      .map((_, rowIndex) =>
        new Array(props.columns)
          .fill(false)
          .map((_, colIndex) => rowIndex * props.columns + colIndex + 1),
      ),
  );

  const columns = computed(() => props.columns);
  const boxWidth = ref('50px');

  const isMouseDown = ref(false);
  const selectedList = ref<string[]>([]);
  const handleMouseDown = ({ row, col }: { row: number; col: number }) => {
    isMouseDown.value = true;
    selectedList.value = [`${row}_${col}`];
  };

  const handleMouseEnter = ({
    row: endRow,
    col: endCol,
  }: {
    row: number;
    col: number;
  }) => {
    if (isMouseDown.value) {
      const [startRow, startCol] = selectedList.value[0].split('_').map(Number);
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
          selectedList.value.push(`${i}_${j}`);
        }
      }
    }
  };

  const handleMouseUp = () => {
    isMouseDown.value = false;
  };
</script>

<template>
  <div @mouseup="handleMouseUp">
    <div class="grid" v-for="(columns, rowIndex) in gridState" :key="rowIndex">
      <div
        class="box"
        v-for="(val, colIndex) in columns"
        :class="{
          selected: selectedList.includes(`${rowIndex + 1}_${colIndex + 1}`),
        }"
        @mousedown="handleMouseDown({ row: rowIndex + 1, col: colIndex + 1 })"
        @mouseenter="handleMouseEnter({ row: rowIndex + 1, col: colIndex + 1 })"
      >
        {{ val }}
      </div>
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
    margin: 0.1rem 0.25rem;
  }
  .selected {
    background-color: #ccc;
  }
</style>
