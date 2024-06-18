<template>
  <div class="cells-container">
    <div
      class="cell"
      v-for="cell in numberOfCells"
      @contextmenu.prevent="$emit('countActionsEvent')"
      @click="$emit('countActionsEvent')"
    ></div>
  </div>
</template>

<script setup>
defineProps({
  numberOfCells: Number,
});

defineEmits(["countActionsEvent"]);
</script>

<style scoped>
.cells-container {
  display: flex;
  flex-wrap: wrap;
}

.cell {
  width: 72px;
  height: 72px;
  border: 1px solid #ccc;
  background-color: rgb(133, 127, 127);
}

.cell:hover {
  cursor: pointer;
  transform: scale(1.2);
  border: 1px solid rgba(0, 0, 0, 0.5);
  filter: blur(1px);
  background-color: rgb(136, 134, 134);
}
</style>
