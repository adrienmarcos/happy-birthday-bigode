<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import confetti from "canvas-confetti";
import clapsAudio from "../assets/claps.mp3";
import levelupAudio from "../assets/levelup.mp3";
import rodrigoFone from "../assets/rodrigo-fone.jpeg";
import rodrigoNuvens from "../assets/rodrigo-nuvens.jpeg";

const emit = defineEmits(["play-audio"]);

const is8Bit = ref(false);
const showGift = ref(false);

let audio = null;

const triggerConfetti = () => {
  const duration = 3 * 1000;
  const animationEnd = Date.now() + duration;
  const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 };

  const randomInRange = (min, max) => Math.random() * (max - min) + min;

  const interval = setInterval(() => {
    const timeLeft = animationEnd - Date.now();

    if (timeLeft <= 0) {
      return clearInterval(interval);
    }

    const particleCount = 50 * (timeLeft / duration);
    confetti({
      ...defaults,
      particleCount,
      origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 },
    });
    confetti({
      ...defaults,
      particleCount,
      origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 },
    });
  }, 250);
};

const openGift = () => {
  const levelup = new Audio(levelupAudio);
  levelup.play();
  emit("play-audio");
  showGift.value = true;
};

onMounted(() => {
  // Disparar confetes
  triggerConfetti();

  // Play audio
  audio = new Audio(clapsAudio);
  audio.play();
  setTimeout(() => {
    if (audio) {
      audio.pause();
      audio.currentTime = 0;
    }
  }, 7000);

  // Easter Egg no Console
  console.log(
    "%c[AILOG-SYS] %cAcesso root concedido ao aniversariante. %cFeliz aniversário, Rodrigo!",
    "color: #2ecc71; font-weight: bold;",
    "color: #ffffff;",
    "color: #2ecc71; font-style: italic;",
  );
});

onUnmounted(() => {
  if (audio) {
    audio.pause();
    audio.currentTime = 0;
    audio = null;
  }
});
</script>

<template>
  <div class="dashboard-container flex flex-center q-pa-md">
    <div class="content-wrapper text-center">
      <!-- Cabeçalho com Avatar -->
      <div class="avatar-section q-mb-xl">
        <q-avatar
          size="150px"
          class="avatar-neon"
          :class="{ 'pixelated-mode': is8Bit }"
        >
          <img :src="rodrigoFone" alt="Rodrigo de Fone" />
        </q-avatar>
        <h1 class="text-h3 text-bold q-mt-md text-primary title-glow">
          Feliz Aniversário,<br />
          <span class="text-white">Rodrigo Garcia!</span>
        </h1>
      </div>

      <!-- Card Central Glassmorphism -->
      <q-card class="glass-card q-pa-lg q-mb-xl">
        <q-card-section>
          <p class="text-body1 message-text">
            Que neste novo ciclo sua vida tenha a estabilidade de uma aplicação
            <strong>Spring Boot</strong> bem configurada e que suas rotas sejam
            sempre otimizadas, sem pedágios ou trânsito! Que a cobertura de
            testes da sua felicidade seja de 100% e que os
            <strong>NullPointerExceptions</strong> fiquem bem longe.
            <br /><br />
            E agora que a arquitetura da família escalou e o deploy da
            <strong>versão 2.0</strong> foi um sucesso (com a v1.0 já rodando há
            mais de uma década!), aproveite muito a licença-paternidade. Que não
            falte memória RAM — nem café — para dar conta desse novo bebê!
            <br /><br />
            Parabéns, bigode!
          </p>
        </q-card-section>
      </q-card>

      <!-- Área de Brincadeiras -->
      <div class="actions-section">
        <q-btn-group rounded outline>
          <!-- <q-btn
            :color="is8Bit ? 'secondary' : 'white'"
            outline
            label="Modo 8-bits"
            @click="is8Bit = !is8Bit"
            icon="videogame_asset"
          /> -->
          <q-btn
            color="primary"
            label="Abrir Presente"
            @click="openGift"
            icon="card_giftcard"
          />
        </q-btn-group>
      </div>
    </div>

    <!-- Modal do Presente -->
    <q-dialog v-model="showGift" backdrop-filter="blur(4px)">
      <q-card
        class="bg-dark text-white glass-card"
        style="width: 400px; max-width: 90vw"
      >
        <q-img :src="rodrigoNuvens">
          <div
            class="absolute-bottom text-subtitle1 text-center bg-black-opacity"
          >
            O limite é a nuvem! (E os servidores da AWS, claro).
          </div>
        </q-img>
        <q-card-actions align="right">
          <q-btn flat label="Fechar" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<style scoped>
.dashboard-container {
  min-height: 100vh;
  background: radial-gradient(circle at center, #1e1e1e 0%, #121212 100%);
}

.content-wrapper {
  max-width: 800px;
  width: 100%;
}

.avatar-neon {
  border: 3px solid #2ecc71;
  box-shadow:
    0 0 20px rgba(46, 204, 113, 0.4),
    0 0 40px rgba(46, 204, 113, 0.2);
  transition: all 0.3s ease;
}

.pixelated-mode {
  image-rendering: pixelated;
  filter: contrast(1.2) grayscale(0.5) sepia(1) hue-rotate(80deg);
}

.title-glow {
  text-shadow: 0 0 10px rgba(46, 204, 113, 0.3);
}

.glass-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

.message-text {
  line-height: 1.8;
  font-size: 1.1rem;
}

.bg-black-opacity {
  background: rgba(0, 0, 0, 0.6);
}

@media (max-width: 600px) {
  .text-h3 {
    font-size: 2rem;
  }
}
</style>
