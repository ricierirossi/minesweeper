<script setup>
import "./assets/App.css";

import CellsComponent from "./components/CellsComponent.vue";
import ScoredBoard from "./components/ScoreBoard.vue";

import { onMounted, ref } from "vue";

const rows = ref(10);
const column = ref(10);
const bombs = ref(10);
const cells = ref([]);
const numberOfCells = rows.value * column.value;
const actionsCounter = ref(0);
const time = ref({ hours: 0, minutes: 0, seconds: 0, maxValueOfTime: 60 });

function initializeCells() {
  for (let cellIndex = 0; cellIndex < numberOfCells; cellIndex++) {
    cells.value.push({
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
  setInterval(() => {
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

onMounted(() => {
  initializeCells();
});
</script>

<template>
  <body>
    <div class="game">
      <ScoredBoard :actions-counter="actionsCounter" :time="time" />
      <CellsComponent
        :number-of-cells="numberOfCells"
        :cells="cells"
        @count-actions-event="countActions"
        @start-stopwatch-event.once="stopwatch"
      />
    </div>
  </body>
</template>
