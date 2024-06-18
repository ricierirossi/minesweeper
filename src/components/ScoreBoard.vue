<template>
  <div class="container">
    <p>💣 10</p>
    <p>😄</p>
    <div class="status">
      <p>⏳ 01:32</p>
      <p>{{ actionsCounter }} actions</p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  actionsCounter: Number,
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
