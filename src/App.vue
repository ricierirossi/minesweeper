<script setup>
import "./assets/App.css";
import BoardComponent from "./components/BoardComponent.vue";
import ScoredBoard from "./components/ScoreBoard.vue";
</script>

<template>
  <body>
    <div class="game">
      <BoardComponent />
      <ScoredBoard />
    </div>
  </body>
</template>
