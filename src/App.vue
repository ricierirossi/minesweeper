<script setup>
import "./assets/App.css";

import BoardComponent from "./components/BoardComponent.vue";
import CellsComponent from "./components/CellsComponent.vue";
import ScoredBoard from "./components/ScoreBoard.vue";

import { ref } from "vue";

const rows = ref(10);
const column = ref(10);
const bombs = ref(10);
const cells = ref([]);
const numberOfCells = rows.value * column.value;
const actionsCounter = ref(0);

function initializeCells() {
  for (let cellIndex = 0; cellIndex++; cellIndex <= numberOfCells) {
    cells.push({
      revealed: false,
      flagged: false,
      bomb: false,
      adjacentBombs: 0,
    });
  }
}

function countActions() {
  actionsCounter.value += 1;
}
</script>

<template>
  <body>
    <div class="game">
      <BoardComponent />
      <ScoredBoard :actions-counter="actionsCounter" />
      <CellsComponent
        :number-of-cells="numberOfCells"
        @count-actions-event="countActions"
      />
    </div>
  </body>
</template>
