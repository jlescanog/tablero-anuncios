<script setup>
import { ref, onMounted } from 'vue';
import Menu from './components/Menu.vue';
import DocumentViewer from './components/DocumentViewer.vue';

// --- ESTADO ---
const documentoActual = ref(null);
const tituloActual = ref('');

// --- FUNCIONES ---
const abrirDocumento = (item) => {
  tituloActual.value = item.texto;
  documentoActual.value = item.url;
  
  // Añadir entrada al historial para que el botón "atrás" funcione
  window.history.pushState({ documento: true }, '');
  window.scrollTo(0, 0);
};

const volverAlMenu = () => {
  documentoActual.value = null;
  tituloActual.value = '';
  // Si volvemos manualmente, nos aseguramos de no dejar estados huérfanos
  if (window.history.state?.documento) {
    window.history.back();
  }
};

// Escuchar el botón atrás del navegador
onMounted(() => {
  window.onpopstate = (event) => {
    if (!event.state?.documento) {
      documentoActual.value = null;
      tituloActual.value = '';
    }
  };
});
</script>

<template>
  <main class="contenedor-principal">
    <Transition name="fade" mode="out-in">
      <Menu 
        v-if="!documentoActual" 
        @abrir-documento="abrirDocumento" 
      />
      
      <DocumentViewer 
        v-else 
        :titulo="tituloActual" 
        :url="documentoActual" 
        @volver="volverAlMenu" 
      />
    </Transition>
  </main>
</template>

<style scoped>
:root {
  --color-fondo: #f8f9fa;
  --color-primario: #2c3e50;
  --sombra: 0 4px 20px rgba(0,0,0,0.08);
}

.contenedor-principal {
  font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  min-height: 100vh;
  background-color: #f8f9fa;
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  border-radius: 16px; 
  overflow: hidden;
  position: relative;
}

/* Transiciones globales */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>