<template>
  <div
    class="cells-container"
    :style="{
      'grid-template-columns': 'repeat(' + columns + ', 72px)',
      'grid-template-rows': 'repeat(' + rows + ', 72px)',
    }"
  >
    <div
      class="cell"
      v-for="(cell, index) of cells"
      :key="index"
      @click="handleLeftClick(cell, index, rows, columns)"
      @contextmenu.prevent="handleRightClick(cell, bombs)"
    >
      {{ cell.content }}
    </div>
  </div>
</template>

<script setup>
import { onUpdated, ref } from "vue";

const flaggedCells = ref(0);

const props = defineProps({
  columns: Number,
  rows: Number,
  numberOfCells: Number,
  cells: Array,
  bombs: Number,
  flaggedCells: Number,
  newGame: Boolean,
});

const emit = defineEmits([
  "countActionsEvent",
  "startStopwatchEvent",
  "isNewGameEvent",
  "remainingBombsEvent",
]);

function handleLeftClick(cell, index, rows, columns) {
  startGame();
  revealCell(cell, index, rows, columns);
  // calculateAdjacentBombs(cell, index, rows, columns);
}

function handleRightClick(cell, bombs) {
  startGame();
  flagCell(cell, bombs);
}

function startGame() {
  emit("countActionsEvent");
  emit("startStopwatchEvent");
  emit("isNewGameEvent");
}

function revealCell(cell, index, rows, columns) {
  if (cell.revealed === false) {
    calculateAdjacentBombs(cell, index, rows, columns);
    cell.revealed = true;
  }
  if (cell.bomb) {
    cell.content = "💣";
  }
  if (cell.bomb === false) {
    cell.content = cell.adjacentBombs;
  }
}

function flagCell(cell, bombs) {
  if (!cell.revealed && !cell.flagged && flaggedCells.value < bombs) {
    cell.flagged = true;
    cell.content = "🚩";
    flaggedCells.value += 1;
  } else if (cell.flagged) {
    cell.flagged = false;
    cell.content = "";
    flaggedCells.value -= 1;
  }
  emit("remainingBombsEvent", flaggedCells.value);
}

function resetFlags() {
  if (props.newGame) {
    flaggedCells.value = 0;
  }
}

function calculateAdjacentBombs(cell, index, rows, columns) {
  const adjacentCells = getAdjacentIndexes(index, rows, columns);
  adjacentCells.forEach((adjacentCell) => {
    if (props.cells[adjacentCell].bomb) {
      cell.adjacentBombs += 1;
    }
  });
}

function getAdjacentIndexes(index, rows, columns) {
  const currentRow = Math.floor(index / rows);
  const currentCol = index % columns;
  const adjacentIndexes = [];

  const directions = [
    [-1, -1],
    [-1, 0],
    [-1, 1],
    [0, -1],
    [0, 1],
    [1, -1],
    [1, 0],
    [1, 1],
  ];

  directions.forEach((direction) => {
    const newRow = currentRow + direction[0];
    const newCol = currentCol + direction[1];

    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < columns) {
      adjacentIndexes.push(newRow * rows + newCol);
    }
  });
  return adjacentIndexes;
}

onUpdated(() => {
  resetFlags();
});
</script>

<style scoped>
.cells-container {
  display: grid;
}

.cell {
  border: 1px solid #ccc;
  background-color: rgb(133, 127, 127);
  display: flex;
  justify-content: center;
  align-items: center;
}

.cell:hover {
  cursor: pointer;
  transform: scale(1.2);
  border-color: rgba(0, 0, 0, 0.5);
  background-color: rgb(136, 134, 134);
}
</style>
