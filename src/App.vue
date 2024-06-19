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
const time = ref({ hours: 0, minutes: 0, seconds: 0, maxValueOfTime: 60 });

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
</script>

<template>
  <body>
    <div class="game">
      <BoardComponent />
      <ScoredBoard :actions-counter="actionsCounter" :time="time" />
      <CellsComponent
        :number-of-cells="numberOfCells"
        @count-actions-event="countActions"
        @click.once="stopwatch"
      />
    </div>
  </body>
</template>
