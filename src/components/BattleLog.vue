<template>
  <section class="w-100">
    <h2>Battle Log</h2>
    <ul class="list-unstyled" style="overflow-y: auto; height: 200px">
      <li v-for="msg in props.logMsg.arr" class="my-2">
        <span
          :class="{
            'log--player': msg.actBy === 'player',
            'log--monster': msg.actBy === 'monster',
          }"
          >{{ msg.actBy === "player" ? "Player" : "Monster" }}</span
        >
        <span v-if="msg.actType === 'heal'">
          Heal <span class="log--heal">{{ msg.actVal }}hp</span>
        </span>
        <span v-else-if="msg.actType === 'sp'">
          Special Attack
          <span class="log--damage">{{ msg.actVal }}hp</span></span
        >
        <span v-else>
          Attack <span class="log--damage">{{ msg.actVal }}hp</span></span
        >
      </li>
    </ul>
  </section>
</template>

<script setup>
const props = defineProps({ logMsg: Object });
</script>
