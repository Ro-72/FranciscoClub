<script setup>
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faComments, faPaperPlane, faXmark } from '@fortawesome/free-solid-svg-icons'

const isOpen = ref(false)

function toggleChat() {
  isOpen.value = !isOpen.value
}
</script>

<template>
  <div class="chatbot-widget" :class="{ 'chatbot-widget--open': isOpen }">
    <section
      v-if="isOpen"
      id="chatbot-panel"
      class="chatbot-panel"
      aria-labelledby="chatbot-title"
    >
      <header class="chatbot-header">
        <div class="chatbot-avatar" aria-hidden="true">
          <FontAwesomeIcon :icon="faComments" />
        </div>
        <div>
          <p class="chatbot-label">Francisco’s Club</p>
          <h2 id="chatbot-title">Asistente del club</h2>
          <span class="chatbot-status"><i></i> Disponible para ayudarte</span>
        </div>
        <button
          class="chatbot-close"
          type="button"
          aria-label="Cerrar asistente visual"
          @click="toggleChat"
        >
          <FontAwesomeIcon :icon="faXmark" aria-hidden="true" />
        </button>
      </header>

      <div class="chatbot-messages" aria-live="polite">
        <div class="chatbot-message chatbot-message--bot">
          Hola. Estamos preparando este espacio para acompañarte dentro de Francisco’s Club.
        </div>
        <p class="chatbot-note">Por ahora esta conversación es solo una vista previa.</p>
      </div>

      <div class="chatbot-suggestions" aria-label="Sugerencias de conversación">
        <button type="button" disabled>Precio de la cancha</button>
        <button type="button" disabled>Cómo llegar</button>
        <button type="button" disabled>Conoce el club</button>
      </div>

      <form class="chatbot-composer" @submit.prevent>
        <input type="text" placeholder="Escribe un mensaje..." aria-label="Mensaje" disabled />
        <button type="submit" aria-label="Enviar mensaje" disabled>
          <FontAwesomeIcon :icon="faPaperPlane" aria-hidden="true" />
        </button>
      </form>
    </section>

    <button
      class="chatbot-trigger"
      type="button"
      :aria-expanded="isOpen"
      aria-controls="chatbot-panel"
      :aria-label="isOpen ? 'Cerrar asistente visual' : 'Abrir asistente visual'"
      @click="toggleChat"
    >
      <FontAwesomeIcon :icon="isOpen ? faXmark : faComments" aria-hidden="true" />
      <span v-if="!isOpen" class="chatbot-trigger-label">¿Hablamos?</span>
    </button>
  </div>
</template>

<style scoped>
.chatbot-widget {
  --chat-cream: #f4f0e6;
  --chat-black: #101010;
  --chat-gold: #d5a32a;
  --chat-green: #173f35;
  bottom: 24px;
  position: fixed;
  right: 24px;
  z-index: 1001;
}

.chatbot-trigger {
  align-items: center;
  background: var(--chat-black);
  border: 1px solid rgba(244, 240, 230, 0.24);
  border-radius: 100px;
  box-shadow: 0 12px 32px rgba(16, 16, 16, 0.2);
  color: var(--chat-cream);
  display: flex;
  font-size: 21px;
  gap: 11px;
  height: 58px;
  justify-content: center;
  padding: 0 18px;
  transition: background-color 0.25s ease, color 0.25s ease, transform 0.25s ease;
}

.chatbot-trigger:hover,
.chatbot-trigger:focus-visible {
  background: var(--chat-gold);
  color: var(--chat-black);
  transform: translateY(-3px);
}

.chatbot-trigger:focus-visible {
  outline: 3px solid var(--chat-gold);
  outline-offset: 4px;
}

.chatbot-trigger-label {
  font-family: 'DM Mono', monospace;
  font-size: 8px;
  letter-spacing: 0.13em;
  text-transform: uppercase;
}

.chatbot-panel {
  background: var(--chat-cream);
  border: 1px solid rgba(16, 16, 16, 0.18);
  bottom: 72px;
  box-shadow: 0 20px 60px rgba(16, 16, 16, 0.2);
  color: var(--chat-black);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  position: absolute;
  right: 0;
  width: min(360px, calc(100vw - 32px));
}

.chatbot-header {
  align-items: center;
  background: var(--chat-green);
  color: var(--chat-cream);
  display: flex;
  gap: 12px;
  padding: 19px 18px;
}

.chatbot-avatar {
  align-items: center;
  border: 1px solid rgba(244, 240, 230, 0.4);
  border-radius: 50%;
  color: var(--chat-gold);
  display: flex;
  flex: 0 0 38px;
  height: 38px;
  justify-content: center;
}

.chatbot-label {
  color: var(--chat-gold);
  font-family: 'DM Mono', monospace;
  font-size: 8px;
  letter-spacing: 0.13em;
  margin: 0 0 4px;
  text-transform: uppercase;
}

.chatbot-header h2 {
  font-family: 'Manrope', sans-serif;
  font-size: 16px;
  letter-spacing: -0.04em;
  line-height: 1;
  margin: 0 0 7px;
}

.chatbot-status {
  align-items: center;
  color: rgba(244, 240, 230, 0.6);
  display: flex;
  font-size: 9px;
  gap: 6px;
}

.chatbot-status i {
  background: #7cba67;
  border-radius: 50%;
  display: block;
  height: 6px;
  width: 6px;
}

.chatbot-close {
  align-self: flex-start;
  background: transparent;
  border: 0;
  color: rgba(244, 240, 230, 0.7);
  font-size: 16px;
  margin-left: auto;
  padding: 2px;
}

.chatbot-close:hover,
.chatbot-close:focus-visible {
  color: var(--chat-gold);
}

.chatbot-close:focus-visible {
  outline: 2px solid var(--chat-gold);
  outline-offset: 3px;
}

.chatbot-messages {
  min-height: 132px;
  padding: 22px 18px 8px;
}

.chatbot-message {
  font-size: 12px;
  line-height: 1.7;
  max-width: 280px;
}

.chatbot-message--bot {
  background: #fffdf7;
  border: 1px solid rgba(16, 16, 16, 0.08);
  padding: 13px 14px;
}

.chatbot-note {
  color: #77736b;
  font-family: 'DM Mono', monospace;
  font-size: 8px;
  letter-spacing: 0.06em;
  margin: 12px 0 0;
  text-transform: uppercase;
}

.chatbot-suggestions {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  padding: 8px 18px 18px;
}

.chatbot-suggestions button {
  background: transparent;
  border: 1px solid rgba(16, 16, 16, 0.18);
  border-radius: 100px;
  color: #77736b;
  font-size: 10px;
  padding: 8px 10px;
}

.chatbot-suggestions button:disabled {
  cursor: default;
  opacity: 0.85;
}

.chatbot-composer {
  border-top: 1px solid rgba(16, 16, 16, 0.14);
  display: flex;
  gap: 9px;
  padding: 13px 15px;
}

.chatbot-composer input {
  background: transparent;
  border: 0;
  color: var(--chat-black);
  flex: 1;
  font: 11px 'Manrope', sans-serif;
  min-width: 0;
  outline: 0;
}

.chatbot-composer input::placeholder {
  color: #989286;
}

.chatbot-composer button {
  align-items: center;
  background: var(--chat-gold);
  border: 0;
  border-radius: 50%;
  color: var(--chat-black);
  display: flex;
  height: 32px;
  justify-content: center;
  width: 32px;
}

.chatbot-composer button:disabled {
  cursor: default;
  opacity: 0.55;
}

@media (max-width: 680px) {
  .chatbot-widget {
    bottom: 84px;
    right: 16px;
  }

  .chatbot-trigger {
    height: 52px;
    padding: 0;
    width: 52px;
  }

  .chatbot-trigger-label {
    display: none;
  }

  .chatbot-panel {
    bottom: 64px;
    right: -2px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .chatbot-trigger {
    transition-duration: 0.01ms !important;
  }
}
</style>
