<script setup lang="ts">
const emit = defineEmits<{
  (e: "openRules"): void;
  (e: "openStats"): void;
}>();
</script>

<template>
  <teleport to="#app-header">
    <button @click.prevent="() => emit('openRules')">
      <svg
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        height="32"
        viewBox="0 0 32 32"
        width="32"
        class="game-icon"
      >
        <path
          fill="var(--color-tone-1)"
          d="M15 24H17.6667V21.3333H15V24ZM16.3333 2.66666C8.97333 2.66666 3 8.63999 3 16C3 23.36 8.97333 29.3333 16.3333 29.3333C23.6933 29.3333 29.6667 23.36 29.6667 16C29.6667 8.63999 23.6933 2.66666 16.3333 2.66666ZM16.3333 26.6667C10.4533 26.6667 5.66667 21.88 5.66667 16C5.66667 10.12 10.4533 5.33332 16.3333 5.33332C22.2133 5.33332 27 10.12 27 16C27 21.88 22.2133 26.6667 16.3333 26.6667ZM16.3333 7.99999C13.3867 7.99999 11 10.3867 11 13.3333H13.6667C13.6667 11.8667 14.8667 10.6667 16.3333 10.6667C17.8 10.6667 19 11.8667 19 13.3333C19 16 15 15.6667 15 20H17.6667C17.6667 17 21.6667 16.6667 21.6667 13.3333C21.6667 10.3867 19.28 7.99999 16.3333 7.99999Z"
        ></path>
      </svg>
      Rules
    </button>
    <div>
      <h1 style="margin: 0; font-size: 1.6rem">Wordle</h1>
      <!-- <p style="margin: 12px auto 24px">for romanian words</p> -->
    </div>
    <button @click.prevent="() => emit('openStats')">
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
      Stats
    </button>
  </teleport>
</template>

<style>
.game-icon {
    width: 20px;
    height: 20px;

    path {
        fill: white;
    }
}

button {
    display: flex;
    gap: 8px;
    align-items: center;
}
</style>