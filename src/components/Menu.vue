<script setup>
import { listaDeBotones } from '../data/buttons.js';

const emit = defineEmits(['abrir-documento']);

const abrirDocumento = (item) => {
  emit('abrir-documento', item);
};
</script>

<template>
  <div class="menu-vista">
    <div class="banner-container">
      <img src="/banner.jpg" alt="Imagen de la congregación" class="banner-img" />
    </div>

    <header class="encabezado">
      <h1>Tablero de Anuncios</h1>
      <p>Congregación Vilcas</p>
    </header>

    <div class="lista-botones">
      <button 
        v-for="item in listaDeBotones" 
        :key="item.id"
        class="boton-anuncio"
        :style="{ backgroundColor: item.color, color: item.textoOscuro ? '#000' : '#fff' }"
        @click="abrirDocumento(item)"
      >
        {{ item.texto }}
      </button>
    </div>
    
    <footer class="pie-pagina">
      <p>Seleccione una opción para ver la información.</p>
    </footer>
  </div>
</template>

<style scoped>
.menu-vista {
  animation: fadeIn 0.5s ease-in-out;
}

/* ESTILOS DEL BANNER */
.banner-container {
  width: 100%;
  height: 180px;
  background-color: #e0e0e0;
}

.banner-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
}

/* ESTILOS DEL ENCABEZADO */
.encabezado {
  text-align: center;
  padding: 2rem 1rem 1rem 1rem;
  background-color: #fff;
}

.encabezado h1 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.8rem;
  font-weight: 800;
  text-transform: uppercase;
}

.encabezado p {
    color: #666;
    margin-top: 0.5rem;
}

/* ESTILOS DE BOTONES */
.lista-botones {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
  padding: 1.5rem;
  background-color: #fff;
}

.boton-anuncio {
  border: none;
  padding: 1.5rem;
  font-size: 1.2rem;
  font-weight: bold;
  border-radius: 12px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0,0,0,0.15);
  transition: transform 0.1s, filter 0.2s;
  text-align: center;
  width: 100%;
}

.boton-anuncio:active {
  transform: scale(0.97);
}

.pie-pagina {
  text-align: center;
  color: #999;
  font-size: 0.9rem;
  padding: 1.5rem;
  background-color: #fff;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
