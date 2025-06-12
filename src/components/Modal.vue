<script setup lang="ts">
import { ref, watch } from "vue";
const dialog = ref<HTMLDialogElement | null>(null);

const props = defineProps<{
  open: boolean;
  small?: boolean;
}>();

const emit = defineEmits<{
  (e: "close"): void;
}>();

function showDialog() {
  dialog.value?.show();
}

function closeDialog() {
  dialog.value?.close();
}

function handleClose() {
  emit("close");
}

watch(
  () => props.open,
  (newVal) => {
    if (newVal) {
      showDialog();
    } else {
      closeDialog();
    }
  }
);
</script>

<template>
  <teleport to="body">
    <dialog :class="`my-dialog ${small ? 'my-dialog--smal' : ''}`" ref="dialog" @close="handleClose" @cancel.prevent>
      <div class="dialog-wrapper" @click.self="handleClose">
        <div class="content">
          <h2>
            <slot name="header"></slot>
            <button type="button" @click="handleClose" aria-label="Close"><svg aria-hidden="true" xmlns="http://www.w3.org/2000/svg" height="20" viewBox="0 0 24 24" width="20" class="game-icon"><path fill="var(--color-tone-1)" d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"></path></svg></button>
          </h2>
          <slot />
        </div>
      </div>
    </dialog>
  </teleport>
</template>

<style scoped>
h2 {
  margin-top: 0;
  margin-bottom: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h2 > button {
  background: transparent;
  border: none;
  padding: 4px;
  cursor: pointer;
  --color-tone-1: white;
  outline: none !important;
}

h2 > button > svg {
  width: 20px;
  height: 20px;
}
h2 > button > svg  path {
  fill: var(--color-tone-1);
}

.my-dialog {
  width: 100%;
  border: none;
  padding: 0;
  height: 100%;
  background: transparent;
}

.my-dialog:focus-visible {
  outline: 0;
  inset: 0;
}

.dialog-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.5);
}

.my-dialog .content {
  padding: 28px;
  text-align: left;
  width: 400px;
  border-radius: 10px;
  background: black;
  border: 2px solid #3498db;
}

.my-dialog.my-dialog--smal .content {
  max-width: 180px;
  width: 180px;
}

.my-dialog.my-dialog--smal .content h2 {
  font-size: 1.2rem;
  font-weight: normal;
  justify-content: center;
  margin: 0;
}
.my-dialog.my-dialog--smal .content h2 > button {
  display: none;
}
</style>
