<script setup lang="ts">

const letters1 = ["Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"];
const letters2 = ["A", "S", "D", "F", "G", "H", "J", "K", "L"];
const letters3 = ["ENTER", "Z", "X", "C", "V", "B", "N", "M", "BACK"];

const props = defineProps<{
  goodLetters?: string[];
  wrongLetters?: string[];
  includedLetters?: string[];
}>();

const emit = defineEmits<{
  (e: "letter", value: string): void;
}>();

function trigger(letter: string) {
  emit("letter", letter);
}

function getKeyColor(letter: string) {
  if (props.goodLetters?.includes(letter)) {
    return "goodLetter";
  } else if (props.includedLetters?.includes(letter)) {
    return "includedLetter";
  } else if (props.wrongLetters?.includes(letter)) {
    return "wrongLetter";
  }
  return "";
}
</script>

<template>
  <div class="keyboard">
    <button
      v-for="letter in letters1"
      :key="letter"
      type="button"
      :class="getKeyColor(letter)"
      @click.prevent="trigger(letter)"
    >
      {{ letter }}
    </button>
  </div>
  <div class="keyboard" style="margin-top: 6px">
    <span class="spacer"></span>
    <button
      v-for="letter in letters2"
      :key="letter"
      type="button"
      :class="getKeyColor(letter)"
      @click.prevent="trigger(letter)"
    >
      {{ letter }}
    </button>
    <span class="spacer"></span>
  </div>
  <div class="keyboard" style="margin-top: 6px">
    <button
      v-for="letter in letters3"
      :key="letter"
      type="button"
      :class="getKeyColor(letter)"
      @click.prevent="trigger(letter)"
    >
      <span v-if="letter === 'BACK'">
        <svg
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          height="20"
          viewBox="0 0 24 24"
          width="20"
          class="game-icon"
        >
          <path
            fill="var(--color-tone-1)"
            d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H7.07L2.4 12l4.66-7H22v14zm-11.59-2L14 13.41 17.59 17 19 15.59 15.41 12 19 8.41 17.59 7 14 10.59 10.41 7 9 8.41 12.59 12 9 15.59z"
          ></path>
        </svg>
      </span>
      <span v-else-if="letter === 'ENTER'" style="font-size: 0.8rem">
        {{ letter }}
      </span>
      <span v-else>
        {{ letter }}
      </span>
    </button>
  </div>
</template>

<style scoped>
.keyboard {
  margin-top: 32px;
  display: flex;
  justify-content: center;
  gap: 8px;
  min-width: 60vw;
}

.keyboard > button {
  /* padding: 12px 14px; */
  height: 58px;
  flex: 1;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
}

.keyboard > button.wrongLetter {
  background-color: gray;
  color: black;
}

.keyboard > button.goodLetter {
  background-color: greenyellow;
  color: black;
}

.keyboard > button.includedLetter {
  background-color: yellow;
  color: black;
}

.keyboard > .spacer {
  height: 58px;
  flex: 0.5;
  padding: 0;
}
</style>
