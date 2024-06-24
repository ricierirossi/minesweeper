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

const emit = defineEmits([
  "countActionsEvent",
  "startStopwatchEvent",
  "isNewGameEvent",
]);

function handleLeftClick(cell) {
  startGame();
  revealCell(cell);
}

function handleRightClick(cell) {
  startGame();
  flagCell(cell);
}

function startGame() {
  emit("countActionsEvent");
  emit("startStopwatchEvent");
  emit("isNewGameEvent");
}

function revealCell(cell) {
  if (!cell.revealed) {
    cell.revealed = !cell.revealed;
  }
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
  display: flex;
  justify-content: center;
  align-items: center;
}

.cell:hover {
  cursor: pointer;
  transform: scale(1.2);
  border: 1px solid rgba(0, 0, 0, 0.5);
  background-color: rgb(136, 134, 134);
}
</style>
