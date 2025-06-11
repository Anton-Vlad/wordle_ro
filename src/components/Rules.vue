<script setup lang="ts">
import Modal from "./Modal.vue";

const props = defineProps<{
  open: boolean;
}>();

const emit = defineEmits<{
  (e: 'close'): void;
}>();

function handleClose() {
  emit("close");
}

</script>

<template>
  <Modal :open="open" @close="handleClose">
    <h2>How To Play</h2>
    <p>Guess the Wordle in 6 tries.</p>

    <section>
      <ul>
        <li>Each guess must be a valid 5-letter word.</li>
        <li>
          The color of the tiles will change to show how close your guess was to
          the word.
        </li>
      </ul>
      <h3>Examples</h3>
      <div class="wordle-line">
        <div class="goodLetter">W</div>
        <div class="">O</div>
        <div class="">R</div>
        <div class="">D</div>
        <div class="">Y</div>
      </div>
      <p><strong>W</strong> is in the word and in the correct spot.</p>

      <div class="wordle-line">
        <div class="">L</div>
        <div class="includedLetter">I</div>
        <div class="">G</div>
        <div class="">H</div>
        <div class="">T</div>
      </div>
      <p><strong>I</strong> is in the word but in the wrong spot.</p>

      <div class="wordle-line">
        <div class="">R</div>
        <div class="">O</div>
        <div class="">G</div>
        <div class="wrongLetter">U</div>
        <div class="">E</div>
      </div>
      <p><strong>U</strong> is not in the word in any spot.</p>
    </section>
  </Modal>
</template>

<style scoped>
h2 {
  margin-top: 0;
  margin-bottom: 4px;
}

p {
  margin: 0 0 24px;
}

ul {
  padding-left: 20px;
}

section p {
  margin: 8px 0 24px 0;
}

section p:last-of-type {
  margin-bottom: 0;
}

.wordle-line {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
}
.wordle-line > div {
  min-width: 40px;
  max-width: 40px;
  height: 40px;
  background-color: aliceblue;
  color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  font-size: 1.4rem;
  border-radius: 4px;
}

.wordle-line > div.wrongLetter {
  background-color: gray;
}

.wordle-line > div.goodLetter {
  background-color: greenyellow;
}

.wordle-line > div.includedLetter {
  background-color: yellow;
}
</style>
