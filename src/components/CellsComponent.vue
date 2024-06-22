<template>
  <div class="cells-container">
    <div
      class="cell"
      v-for="(cell, index) of cells"
      :key="index"
      @click="handleLeftClick(cell)"
      @contextmenu.prevent="handleRightClick(cell)"
    >
      {{ cell.flagged ? "🚩" : "" }}{{ cell.bomb ? "💣" : "" }}
    </div>
  </div>
</template>

<script setup>
defineProps({
  numberOfCells: Number,
  cells: Array,
});

const emit = defineEmits(["countActionsEvent", "startStopwatchEvent"]);

function handleLeftClick(cell) {
  emit("countActionsEvent");
  emit("startStopwatchEvent");
  revealCell(cell);
}

function revealCell(cell) {
  if (!cell.revealed) {
    cell.revealed = !cell.revealed;
  }
}

function handleRightClick(cell) {
  emit("countActionsEvent");
  emit("startStopwatchEvent");
  flagCell(cell);
}

function flagCell(cell) {
  if (!cell.revealed) {
    cell.flagged = !cell.flagged;
  }
}
</script>

<style scoped>
.cells-container {
  display: flex;
  flex-wrap: wrap;
}

.cell {
  width: 72px;
  height: 72px;
  border: 1px solid #ccc;
  background-color: rgb(133, 127, 127);
}

.cell:hover {
  cursor: pointer;
  transform: scale(1.2);
  border: 1px solid rgba(0, 0, 0, 0.5);
  filter: blur(1px);
  background-color: rgb(136, 134, 134);
}
</style>
