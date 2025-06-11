<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import WORDS from '../words'
import Rules from './Rules.vue'
import Score from './Score.vue'
import Header from './Header.vue'
import type { Game } from '../types';

const board = ref(Array(6).fill("").map(() => Array(5).fill("")));
const boardStatuses = ref(Array(6).fill("").map(() => Array(5).fill("")));

const currentWordRow = ref(0);
const currentLetterCol = ref(0);
const target = ref(getRandomWord());

const showRules = ref(false);
const showScore = ref(false);

const outputMessage = ref('');
const tries = ref(0);
const games = ref<Game[]>([]);
const blockInputs = ref(false)

const wordIsComplete = computed(() => {
  return board.value[currentWordRow.value].every(l => l != '')
})
const wordIsFound = computed(() => {
  return boardStatuses.value[currentWordRow.value].every(s => s === 'goodLetter')
})
const lettersFound = computed(() => {
  let found = [];
  for (let r = 0; r < boardStatuses.value.length; r++) {
    found.push(boardStatuses.value[r].filter(l => l === 'goodLetter').length)
  }
  return Math.max(...found);
})


const isValidKey = (key: string, keyCode: number) => {
  // Check for Enter (13), Space (32), Escape (27), Backspace (8)
  if ([8, 13, 32, 27, 49].includes(keyCode)) {
    return true
  }

  return isLetter(key)
}

function saveResult(status: string) {
  blockInputs.value = true;
  games.value.push({
    status: status,
    target: target.value,
    tries: tries.value,
    letters: lettersFound.value
  })
  outputMessage.value = 'You ' + (status === 'W' ? 'Win' : 'Lose');
}

function resetBoard() {
  board.value = Array(6).fill("").map(() => Array(5).fill(""));
  boardStatuses.value = Array(6).fill("").map(() => Array(5).fill(""));

  currentWordRow.value = 0;
  currentLetterCol.value = 0;
  target.value = getRandomWord();

  outputMessage.value = '';
  tries.value = 0;
  blockInputs.value = false;

  console.log('NEW TARGET: ', target.value)
}

function getRandomWord() {
  let randomIndex = Math.floor(Math.random() * (WORDS.length - 1));
  return WORDS[randomIndex]
}

function isSubmitKey(keyCode: number) {
  if ([13, 32].includes(keyCode)) {
    return true;
  }
  return false;
}
function isDeleteKey(keyCode: number) {
  if ([8].includes(keyCode)) {
    return true;
  }
  return false;
}
function isLetter(key: string) {
  if (key.length === 1 && /^[a-z]$/i.test(key)) {
    return true;
  }
  return false;
}

function validateWordInput() {
  const targetWordArray = target.value.split("").map(l => l.toUpperCase());

  for (let i = 0; i < board.value[currentWordRow.value].length; i++) {
    let statusClass = 'wrongLetter';

    if (board.value[currentWordRow.value][i] === targetWordArray[i]) {
      statusClass = 'goodLetter';
    } else if (targetWordArray.includes(board.value[currentWordRow.value][i])) {
      statusClass = 'includedLetter'
    }

    boardStatuses.value[currentWordRow.value][i] = statusClass;
  }
}

const handleKeydown = (event: KeyboardEvent) => {
  if (blockInputs.value) {
    return;
  }

  const key = event.key.toLowerCase()
  const keyCode = event.keyCode || event.which

  outputMessage.value = '';

  // Check if it's a valid key we want to listen for
  if (isValidKey(key, keyCode)) {
    if (keyCode === 27) {
      showScore.value = false;
      setTimeout(() => {
        showRules.value = !showRules.value;
      }, 100)
      return;
    }
    if (keyCode === 49) { // number 1 from the keyboard numerical
      showRules.value = false;
      setTimeout(() => {
        showScore.value = !showScore.value;
      }, 100)
      return;
    }

    if (isDeleteKey(keyCode)) {
      if (currentLetterCol.value > 0) {
        currentLetterCol.value -= 1;
      }
      board.value[currentWordRow.value][currentLetterCol.value] = '';
      return;
    }

    if (isLetter(key) && !wordIsComplete.value) {
      board.value[currentWordRow.value][currentLetterCol.value] = key.toUpperCase();
      currentLetterCol.value += 1;
      return;
    }

    if (isSubmitKey(keyCode) && wordIsComplete.value) {
      // check the word vs the target,  update the clases for the boardStatuses
      validateWordInput()
      tries.value += 1;

      if (wordIsFound.value) {
        saveResult('W')
        setTimeout(() => {
          resetBoard()
        }, 5000);
        return;
      }

      currentLetterCol.value = 0;
      currentWordRow.value += 1;

      if (currentWordRow.value > 5) {
        saveResult('L')
        setTimeout(() => {
          resetBoard()
        }, 5000);
        return;
      }

      return;
    }

    if (isSubmitKey(keyCode) && !wordIsComplete.value) {
      outputMessage.value = 'Word is incomplet.'
      return;
    }

    if (wordIsComplete.value) {
      outputMessage.value = 'Word is complet. Please submit it. (SPACE or ENTER key)'
    }
  }
}

onMounted(() => {
  document.addEventListener('keydown', handleKeydown)
  console.log("Target: " + target.value)
})

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeydown)
})

function onCloseRules() {
  showRules.value = false;
}

function onCloseScore() {
  showScore.value = false;
}

function onOpenRules() {
  showRules.value = true;
}

function onOpenStats() {
  showScore.value = true;
}
</script>

<template>
  <Header @openRules="onOpenRules" @openStats="onOpenStats" />
  <Rules v-if="!showScore" :open="showRules" @close="onCloseRules" />
  <Score v-if="!showRules" :open="showScore" :games="games" @close="onCloseScore" />

  <div class="panel">
    <p>Tries: {{ tries }}</p>
    <p>Letters found: {{ lettersFound }}</p>
  </div>

  <div class="board">
    <div v-for="(row, index) in board" :key="'row-' + index" class="row">
      <div v-for="(col, colIndex) in row" :key="'col-' + index" class="cell"
        :class="[boardStatuses[index][colIndex], (index === currentWordRow && colIndex === currentLetterCol ? 'pulse-box' : '')]">
        {{ col }}
      </div>
    </div>
  </div>

  <!-- <p class="display">
    {{ outputMessage }}
  </p> -->

</template>

<style scoped>
.panel {
  display: flex;
  justify-content: space-between;
  max-width: 336px;
  margin: 0 auto;
}

.display {
  min-height: 80px;
  max-width: 336px;
  margin: 0 auto;
  margin-top: 24px;
}

.board {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.row {
  display: flex;
  gap: 4px;
  justify-content: center;
}

.cell {
  min-width: 64px;
  max-width: 64px;
  height: 64px;
  background-color: aliceblue;
  color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  font-size: 2rem;
  border-radius: 4px;
}

.cell.empty {
  background-color: aqua;
}

.cell.wrongLetter {
  background-color: gray;
}

.cell.goodLetter {
  background-color: greenyellow;
}

.cell.includedLetter {
  background-color: yellow;
}

.pulse-box {
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0% {
    background-color: aliceblue;
  }

  50% {
    background-color: #3498db;
  }

  100% {
    background-color: aliceblue;
  }
}

</style>
