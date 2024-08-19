<script setup>
import "./assets/App.css";

import CellsComponent from "./components/CellsComponent.vue";
import ScoredBoard from "./components/ScoreBoard.vue";

import { onMounted, ref } from "vue";

const rows = ref(10);
const columns = ref(10);
const bombs = ref(10);
const remainingBombs = ref(bombs.value);
const cells = ref([]);
const numberOfCells = rows.value * columns.value;
const newGame = ref(true);
const actionsCounter = ref(0);
const time = ref({ hours: 0, minutes: 0, seconds: 0, maxValueOfTime: 60 });
const intervalID = ref(0);

function initializeCells() {
  for (let cellIndex = 0; cellIndex < numberOfCells; cellIndex++) {
    cells.value.push({
      content: "",
      revealed: false,
      flagged: false,
      bomb: false,
      adjacentBombs: 0,
      cellID: cellIndex,
    });
  }
  placeBombs();
}

function placeBombs() {
  let placedBombs = 0;
  while (placedBombs < bombs.value) {
    let chosenCellID = Math.floor(Math.random() * numberOfCells);
    if (!cells.value[chosenCellID].bomb) {
      cells.value[chosenCellID].bomb = true;
      placedBombs++;
    }
  }
}

function countActions() {
  actionsCounter.value += 1;
}

function stopwatch() {
  if (newGame.value) {
    intervalID.value = setInterval(() => {
      time.value.seconds++;

      if (time.value.seconds === time.value.maxValueOfTime) {
        time.value.seconds = 0;
        time.value.minutes++;
      }

      if (time.value.minutes === time.value.maxValueOfTime) {
        time.value.minutes = 0;
        time.value.hours++;
      }
    }, 1000);
  }
}

function countRemainingBombs(flaggedCells) {
  remainingBombs.value = bombs.value - flaggedCells;
}

function startNewGame() {
  newGame.value = true;
  cells.value.forEach((cell) => {
    cell.content = "";
    cell.revealed = false;
    cell.flagged = false;
    cell.bomb = false;
    cell.adjacentBombs = 0;
  });
  actionsCounter.value = 0;
  bombs.value = 10;
  remainingBombs.value = bombs.value;
  placeBombs();
  clearInterval(intervalID.value);
  time.value = { hours: 0, minutes: 0, seconds: 0, maxValueOfTime: 60 };
}

onMounted(() => {
  initializeCells();
});
</script>

<template>
  <body>
    <div class="game">
      <ScoredBoard
        :actions-counter="actionsCounter"
        :time="time"
        :remaining-bombs="remainingBombs"
        @new-game-event="startNewGame"
      />
      <CellsComponent
        :columns="columns"
        :rows="rows"
        :number-of-cells="numberOfCells"
        :cells="cells"
        :bombs="bombs"
        :new-game="newGame"
        @count-actions-event="countActions"
        @start-stopwatch-event="stopwatch"
        @new-game-event="startNewGame"
        @is-new-game-event="newGame = false"
        @remaining-bombs-event="
          (flaggedCells) => countRemainingBombs(flaggedCells)
        "
      />
    </div>
  </body>
</template>
