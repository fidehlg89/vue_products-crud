<template>
  <Transition name="fade">
    <div v-if="message" class="toast-container position-fixed bottom-0 end-0 p-4" style="z-index: 1100;">
      <div class="toast show align-items-center text-white border-0 shadow-lg" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="d-flex p-2">
          <div class="toast-body d-flex align-items-center flex-grow-1">
            <i class="bi bi-check-circle-fill me-2 fs-5 text-accent"></i>
            <span class="fw-medium">{{ message }}</span>
          </div>
          <button type="button" class="btn-close btn-close-white me-2 m-auto" aria-label="Close" @click="$emit('clear')"></button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
defineProps({
  message: String
})

defineEmits(['clear'])
</script>

<style scoped>
.toast {
  background: rgba(30, 41, 59, 0.95) !important;
  backdrop-filter: blur(8px);
  border-radius: 12px;
  min-width: 300px;
}

.text-accent {
  color: #10b981;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(30px) scale(0.95);
}
</style>