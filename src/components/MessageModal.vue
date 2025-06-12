<script setup lang="ts">
import Modal from "./Modal.vue";

defineProps<{
  open: boolean;
  message?: string;
}>();

const emit = defineEmits<{
  (e: 'close'): void;
}>();

function handleClose() {
  emit("close");
}
</script>

<template>
  <Modal small :open="open" @close="handleClose">
    <template v-slot:header>
      {{ message }}
    </template>

  </Modal>
</template>

<style scoped>
p {
  margin: 0 0 24px;
}
</style>
