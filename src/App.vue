<script setup>
import { ref } from 'vue';
import Menu from './components/Menu.vue';
import DocumentViewer from './components/DocumentViewer.vue';

// --- ESTADO ---
const documentoActual = ref(null);
const tituloActual = ref('');

// --- FUNCIONES ---
const abrirDocumento = (item) => {
  tituloActual.value = item.texto;
  documentoActual.value = item.url;
  window.scrollTo(0, 0);
};

const volverAlMenu = () => {
  documentoActual.value = null;
  tituloActual.value = '';
};
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
.contenedor-principal {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
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
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>