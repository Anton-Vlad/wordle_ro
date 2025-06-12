<script setup lang="ts">
import Modal from "./Modal.vue";
import { computed } from "vue";
import type { Game } from "../types";

const props = defineProps<{
  open: boolean;
  games: Game[];
}>();

const emit = defineEmits<{
  (e: "close"): void;
}>();

const wins = computed(() => {
  return props.games.filter((x) => x.status === "W").length;
});
const loses = computed(() => {
  return props.games.filter((x) => x.status === "L").length;
});

function handleClose() {
  emit("close");
}
</script>

<template>
  <Modal :open="open" @close="handleClose">
    <template v-slot:header>
      <div>
        <svg
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          height="24"
          viewBox="4 4 24 24"
          width="28"
          class="game-icon"
        >
          <path
            fill="var(--color-tone-1)"
            d="M21.3332 14.6667V4H10.6665V12H2.6665V28H29.3332V14.6667H21.3332ZM13.3332 6.66667H18.6665V25.3333H13.3332V6.66667ZM5.33317 14.6667H10.6665V25.3333H5.33317V14.6667ZM26.6665 25.3333H21.3332V17.3333H26.6665V25.3333Z"
          ></path>
        </svg>
        Score board
      </div>
    </template>

    <table class="score-board">
      <thead>
        <tr>
          <th>W: {{ wins }} / L: {{ loses }}</th>
          <th>Target</th>
          <th>Tries/Words</th>
        </tr>
      </thead>
      <tbody class="table-body">
        <tr v-for="game in games" :key="game.target">
          <td :style="{ color: game.status === 'W' ? 'greenyellow' : 'red' }">
            {{ game.status }}
          </td>
          <td>{{ game.target }}</td>
          <td v-if="game.status === 'W'">
            Won in {{ game.tries }} {{ game.tries > 1 ? "tries" : "try" }}.
          </td>
          <td v-else>Only found {{ game.letters }} / 5 words.</td>
        </tr>

        <tr v-if="!games.length">
          <td>
            <p style="text-align: left; margin: 0">No game completed yet.</p>
          </td>
        </tr>
      </tbody>
    </table>
  </Modal>
</template>

<style scoped>
p {
  margin: 0 0 24px;
}

.score-board {
  width: 100%;
}
.score-board thead tr th {
  border-bottom: 1px solid aliceblue;
}
.score-board tbody tr td:last-child,
.score-board thead tr th:last-child {
  text-align: right;
}

.table-body {
  min-height: 60px;
}
</style>
