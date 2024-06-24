<template>
  <h1>Minesweeper</h1>
  <div class="container">
    <p>💣 10</p>
    <p @click="newGame" style="cursor: pointer">😄</p>
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
defineProps({
  actionsCounter: Number,
  time: Object,
});

const emit = defineEmits(["newGameEvent"]);

function newGame() {
  emit("newGameEvent");
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
