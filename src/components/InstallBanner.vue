<script setup>
import { ref, onMounted } from 'vue';

const mostrarBanner = ref(false);
const plataforma = ref(''); // 'ios' o 'android'
const deferredPrompt = ref(null);

const cerrarBanner = () => {
  mostrarBanner.value = false;
  localStorage.setItem('pwa-banner-dismissed', 'true');
};

const instalarApp = async () => {
  if (deferredPrompt.value) {
    deferredPrompt.value.prompt();
    const { outcome } = await deferredPrompt.value.userChoice;
    if (outcome === 'accepted') {
      cerrarBanner();
    }
    deferredPrompt.value = null;
  }
};

onMounted(() => {
  // 1. Verificar si ya fue cerrado anteriormente
  const yaCerrado = localStorage.getItem('pwa-banner-dismissed');
  if (yaCerrado) return;

  // 2. Detectar si ya está en modo "app" (instalada)
  const isStandalone = window.matchMedia('(display-mode: standalone)').matches || window.navigator.standalone;
  if (isStandalone) return;

  // 3. Detectar Plataforma
  const userAgent = window.navigator.userAgent.toLowerCase();
  const isIos = /iphone|ipad|ipod/.test(userAgent);
  const isAndroid = /android/.test(userAgent);

  if (isIos) {
    plataforma.value = 'ios';
    mostrarBanner.value = true;
  } else if (isAndroid) {
    plataforma.value = 'android';
    // Escuchar el evento de instalación de Chrome/Android
    window.addEventListener('beforeinstallprompt', (e) => {
      e.preventDefault();
      deferredPrompt.value = e;
      mostrarBanner.value = true;
    });
  }
});
</script>

<template>
  <Transition name="slide-up">
    <div v-if="mostrarBanner" class="pwa-banner">
      <div class="banner-contenido">
        <div class="icono-app">📲</div>
        <div class="texto-banner">
          <h3>¡Instala esta App!</h3>
          <p v-if="plataforma === 'ios'">
            Pulsa el botón <strong>Compartir</strong> (cuadrado con flecha) y luego <strong>"Añadir a pantalla de inicio"</strong>.
          </p>
          <p v-else-if="plataforma === 'android'">
            Instala esta web como una aplicación para acceder más rápido.
          </p>
        </div>
      </div>
      
      <div class="acciones-banner">
        <button v-if="plataforma === 'android' && deferredPrompt" @click="instalarApp" class="boton-instalar">
          Instalar Ahora
        </button>
        <button @click="cerrarBanner" class="boton-cerrar">
          Cerrar
        </button>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.pwa-banner {
  position: fixed;
  bottom: 20px;
  left: 20px;
  right: 20px;
  background: white;
  border-radius: 16px;
  padding: 1.2rem;
  box-shadow: 0 10px 30px rgba(0,0,0,0.25);
  z-index: 1000;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  border: 1px solid #eee;
  max-width: 500px;
  margin: 0 auto;
}

.banner-contenido {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.icono-app {
  font-size: 2rem;
}

.texto-banner h3 {
  margin: 0;
  font-size: 1.1rem;
  color: #2c3e50;
}

.texto-banner p {
  margin: 5px 0 0 0;
  font-size: 0.9rem;
  color: #666;
  line-height: 1.3;
}

.acciones-banner {
  display: flex;
  gap: 0.8rem;
}

.boton-instalar {
  background-color: #34a853;
  color: white;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 8px;
  font-weight: bold;
  flex-grow: 1;
  cursor: pointer;
}

.boton-cerrar {
  background-color: #f1f3f4;
  color: #5f6368;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  flex-grow: 1;
}

/* Animación */
.slide-up-enter-active, .slide-up-leave-active {
  transition: transform 0.4s ease, opacity 0.4s ease;
}
.slide-up-enter-from, .slide-up-leave-to {
  transform: translateY(100px);
  opacity: 0;
}
</style>