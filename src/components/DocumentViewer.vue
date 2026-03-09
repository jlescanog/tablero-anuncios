<script setup>
import { ref } from 'vue';

defineProps({
  titulo: {
    type: String,
    required: true
  },
  url: {
    type: String,
    required: true
  }
});

const emit = defineEmits(['volver']);
const cargando = ref(true);

const alCargar = () => {
  cargando.value = false;
};

const volver = () => {
  emit('volver');
};
</script>

<template>
  <div class="documento-vista">
    <div class="barra-navegacion">
      <button @click="volver" class="boton-volver" aria-label="Volver al menú">
        ← Regresar
      </button>
      <span class="titulo-doc">{{ titulo }}</span>
    </div>

    <div v-if="cargando" class="cargando-contenedor">
      <div class="spinner"></div>
      <p>Cargando documento...</p>
    </div>

    <iframe 
      :src="url" 
      class="visor-iframe" 
      :class="{ 'oculto': cargando }"
      frameborder="0" 
      allowfullscreen
      @load="alCargar">
    </iframe>
  </div>
</template>

<style scoped>
.documento-vista {
  display: flex;
  flex-direction: column;
  height: 100vh;
  animation: slideIn 0.3s ease-out;
  background-color: #fff;
}

.barra-navegacion {
  background-color: #2c3e50;
  color: white;
  padding: 0.8rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  z-index: 10;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.boton-volver {
  background-color: #ffffff;
  color: #2c3e50;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 20px;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.1s;
}

.boton-volver:active {
  transform: scale(0.95);
}

.titulo-doc {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 0.9rem;
  font-weight: 500;
}

.cargando-contenedor {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-grow: 1;
  color: #666;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 1rem;
}

.visor-iframe {
  width: 100%;
  flex-grow: 1;
  background: white;
  border: none;
}

.oculto {
  visibility: hidden;
  height: 0;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@keyframes slideIn {
  from { transform: translateX(20px); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}
</style>
