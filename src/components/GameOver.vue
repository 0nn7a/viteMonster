<template>
  <div class="card my-4 w-100 text-center">
    <div class="card-header">
      <h2 class="mb-0">GAME OVER</h2>
    </div>
    <div class="card-body">
      <h4 v-if="props.winner === 'monster'" class="card-title mb-3">
        You LOST this game
      </h4>
      <h4 v-else-if="props.winner === 'player'" class="card-title mb-3">
        You WON this game
      </h4>
      <h4 v-else class="card-title mb-3">It's a DRAW</h4>

      <button
        @click="startAgain"
        class="btn btn-outline-dark rounded-pill px-4 border-2"
      >
        New Game
      </button>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  winner: String,
});
const emit = defineEmits(["startAgain"]);

const startAgain = () => {
  emit("startAgain");
};
</script>
