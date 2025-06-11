<script setup lang="ts">
import { ref, watch } from "vue";
const dialog = ref(null);

const props = defineProps<{
  open: boolean;
}>();

const emit = defineEmits<{
  (e: 'close'): void;
}>();

function showDialog() {
  dialog.value.show();
}

function closeDialog() {
  dialog.value.close();
}

function handleClose(event) {
  console.log("Try to emit from MOdal", event.target);
  emit("close");
}

watch(
  () => props.open,
  (newVal, oldVal) => {
    if (newVal) {
      console.log("PROPS SHOW", dialog.value);
      showDialog();
    } else {
      console.log("PROPS CLOSE", dialog.value);
      closeDialog();
    }
  }
);
</script>

<template>
  <teleport to="body">
    <dialog class="my-dialog" ref="dialog" @close="handleClose" @cancel.prevent >
      <div class="dialog-wrapper" @click.self="handleClose">
        <div class="content">
          <slot />
        </div>
      </div>
    </dialog>
  </teleport>
</template>

<style scoped>
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
</style>
