<template>
  <h1>Minesweeper</h1>
  <div class="container">
    <p>💣 {{ remainingBombs }}</p>
      <p @click="newGame" style="cursor: pointer">
        <div v-if="gameStatus == 'onGoing'">
          😄
        </div>
        <div v-else-if="gameStatus == 'onAction'">
          😮
        </div>
        <div v-else="gameStatus == 'onGameOver'">
          😫
        </div>
      </p>
    <div class="status">
      <p>
        ⏳ {{ time.hours.toString().padStart(2, "0") }}:{{
          time.minutes.toString().padStart(2, "0")
        }}:{{ time.seconds.toString().padStart(2, "0") }}
      </p>
      <p>{{ actionsCounter }} actions</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  actionsCounter: Number,
  time: Object,
  remainingBombs: Number,
});

const emit = defineEmits(["newGameEvent"]);

const gameStatus = ref("onGoing");

function newGame() {
  gameStatus.value = 'onAction'
  emit("newGameEvent");
  gameStatus.value = 'onGoing'
}
</script>
<style scoped>
h1 {
  display: flex;
  justify-content: center;
  width: 100%;
  background-color: #303030;
  color: whitesmoke;
}
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  background-color: #f3f6f4;
}

.status {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
