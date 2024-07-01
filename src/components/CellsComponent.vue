<template>
  <div class="cells-container" :style="{}">
    <div
      class="cell"
      v-for="(cell, index) of cells"
      :key="index"
      @click="handleLeftClick(cell)"
      @contextmenu.prevent="handleRightClick(cell, bombs)"
    >
      {{ cell.content }}
    </div>
  </div>
</template>

<script setup>
import { onUpdated, ref } from "vue";

const props = defineProps({
  columns: Number,
  numberOfCells: Number,
  cells: Array,
  bombs: Number,
  flaggedCells: Number,
  newGame: Boolean,
});

const flaggedCells = ref(0);

const emit = defineEmits([
  "countActionsEvent",
  "startStopwatchEvent",
  "isNewGameEvent",
  "remainingBombsEvent",
]);

function handleLeftClick(cell) {
  startGame();
  revealCell(cell);
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

function revealCell(cell) {
  if (!cell.revealed) {
    cell.revealed = !cell.revealed;
  }
  if (cell.bomb) {
    cell.content = "💣";
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

onUpdated(() => {
  resetFlags();
});
</script>

<style scoped>
/* .cells-container {
  display: flex;
  flex-wrap: wrap;
}*/

.cell {
  border: 1px solid #ccc;
  background-color: rgb(133, 127, 127);
  display: flex;
  justify-content: center;
  align-items: center;
}

.cells-container {
  display: grid;
}

.cell:hover {
  cursor: pointer;
  transform: scale(1.2);
  border: 1px solid rgba(0, 0, 0, 0.5);
  background-color: rgb(136, 134, 134);
}
</style>
