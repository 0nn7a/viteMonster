<template>
  <section class="row w-100 row-cols-1 row-cols-lg-2">
    <div class="col pe-lg-2 px-0">
      <div class="card mb-3 sd">
        <div class="row p-4">
          <div class="col-auto">
            <img
              class="rounded"
              src="../assets/monster.jpg"
              style="width: 120px"
            />
          </div>
          <div class="col">
            <h3>Monster</h3>
            <div class="progress rounded-pill">
              <div
                class="progress-bar bg-danger rounded-pill"
                :style="monsterBarStyles"
              >
                {{ props.monsterHealth }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="col ps-lg-2 px-0">
      <div class="card mb-3 sd">
        <div class="row p-4">
          <div class="col-auto">
            <img
              class="rounded"
              src="../assets/player.jpg"
              style="width: 120px"
            />
          </div>
          <div class="col">
            <h3>Player</h3>
            <div class="progress rounded-pill">
              <div
                class="progress-bar bg-warning rounded-pill text-dark"
                :style="playerBarStyles"
              >
                {{ props.playerHealth }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed } from "@vue/reactivity";

const props = defineProps({
  monsterHealth: Number,
  playerHealth: Number,
});

const monsterBarStyles = computed({
  get() {
    if (props.monsterHealth < 0) {
      return { width: "0%" };
    }
    return { width: props.monsterHealth + "%" };
  },
});
const playerBarStyles = computed({
  get() {
    if (props.playerHealth < 0) {
      return { width: "0%" };
    }
    return { width: props.playerHealth + "%" };
  },
});
</script>
