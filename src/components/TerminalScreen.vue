<script setup>
import { ref, onMounted } from "vue";

const emit = defineEmits(["finished"]);

const lines = [
  "> Iniciando JVM...",
  "> Carregando dependências do Spring Boot...",
  "> Mapeando rotas de frotas da AILOG... [Sucesso]",
  "> Compilando pacote de aniversário do Rodrigo...",
  "> Enviando arquivos para o servidor... [Ambiente de Desenvolvimento offline]",
  "> Subindo alterações direto na produção... [Acesso root concedido]",
  "> Deploy realizado com sucesso. Status Code: 200 OK!",
];

const displayedLines = ref([]);

const typeLine = (line, index, callback) => {
  let charIndex = 0;
  const typingAudio = new Audio("/src/assets/keyboard-typing.mp3");
  typingAudio.loop = true;
  typingAudio.play().catch((e) => console.warn("Audio play blocked:", e));

  const interval = setInterval(() => {
    if (charIndex < line.length) {
      displayedLines.value[index] += line[charIndex];
      charIndex++;
    } else {
      clearInterval(interval);
      typingAudio.pause();
      typingAudio.currentTime = 0;
      callback();
    }
  }, 30);
};

const startBootSequence = async () => {
  for (let i = 0; i < lines.length; i++) {
    displayedLines.value.push("");
    await new Promise((resolve) => typeLine(lines[i], i, resolve));
    await new Promise((resolve) => setTimeout(resolve, 300));
  }

  // Wait a bit after the last line then fade out
  setTimeout(() => {
    emit("finished");
  }, 1500);
};

onMounted(() => {
  startBootSequence();
});
</script>

<template>
  <div class="terminal-screen">
    <div class="terminal-content font-mono q-pa-md">
      <div
        v-for="(line, index) in displayedLines"
        :key="index"
        class="line text-green-5"
      >
        {{ line }}
      </div>
      <span class="cursor">_</span>
    </div>
  </div>
</template>

<style scoped>
.terminal-screen {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background-color: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.terminal-content {
  max-width: 600px;
  width: 100%;
  text-align: left;
  line-height: 1.6;
  font-size: 1.1rem;
}

.cursor {
  animation: blink 1s step-end infinite;
  color: #2ecc71;
}

@keyframes blink {
  from,
  to {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
}

.text-green-5 {
  color: #2ecc71;
}
</style>
