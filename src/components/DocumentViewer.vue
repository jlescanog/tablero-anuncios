<script setup>
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

const volver = () => {
  emit('volver');
};
</script>

<template>
  <div class="documento-vista">
    <div class="barra-navegacion">
      <button @click="volver" class="boton-volver">
        ← Regresar
      </button>
      <span class="titulo-doc">{{ titulo }}</span>
    </div>

    <iframe 
      :src="url" 
      class="visor-iframe" 
      frameborder="0" 
      allowfullscreen>
    </iframe>
  </div>
</template>

<style scoped>
.documento-vista {
  display: flex;
  flex-direction: column;
  height: 100vh;
  animation: slideIn 0.3s ease-out;
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
  transition: background-color 0.2s;
}

.boton-volver:hover {
  background-color: #f0f0f0;
}

.titulo-doc {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 0.9rem;
  font-weight: 500;
}

.visor-iframe {
  width: 100%;
  flex-grow: 1;
  background: white;
}

@keyframes slideIn {
  from { transform: translateX(20px); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}
</style>
