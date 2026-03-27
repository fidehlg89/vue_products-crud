<template>
  <div class="ping-container d-flex flex-column align-items-center justify-content-center">
    <div class="card p-5 border-0 shadow-lg text-center animate-bounce">
      <div class="mb-4">
        <i class="bi bi-cpu fs-1 text-primary"></i>
      </div>
      <h2 class="fw-bold mb-3">Backend Connectivity</h2>
      <p class="text-muted mb-4">Testing connection to the Python Flask server.</p>
      <button 
        type="button" 
        class="btn btn-primary px-4 py-2 fw-semibold shadow-sm"
        @click="getMessage"
      >
        {{ msg || 'Send Ping' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const msg = ref("");

const getMessage = async () => {
  const path = "http://localhost:5001/ping";
  try {
    const res = await axios.get(path);
    msg.value = res.data;
  } catch (error) {
    console.error("Ping error:", error);
    msg.value = "Error connecting to server";
  }
};

onMounted(() => {
  getMessage();
});
</script>

<style scoped>
.ping-container {
  min-height: 60vh;
}

.card {
  border-radius: 20px;
  background: white;
  max-width: 400px;
}

.animate-bounce {
  animation: slideUp 0.6s ease-out;
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
