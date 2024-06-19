<template>
  <div class="container">
    <p>💣 10</p>
    <p>😄</p>
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
</script>
<style scoped>
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
