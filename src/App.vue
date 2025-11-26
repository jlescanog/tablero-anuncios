<script setup>
import { ref } from 'vue';

// --- ESTADO ---
const documentoActual = ref(null);
const tituloActual = ref('');

// --- TUS BOTONES (Versión Colorida) ---
// Actualiza los links con los tuyos reales que terminen en /preview
const listaDeBotones = [
  { 
    id: 1, 
    texto: 'Vida y Ministerio Cristianos', 
    color: '#4285F4', // Azul Google
    url: 'https://docs.google.com/document/d/1r759D7NykShdrWksz6wkgzTeaekriy4EFlw54jbFH0Q/preview?tab=t.0' 
  },
  { 
    id: 2, 
    texto: 'Discursos Públicos', 
    color: '#34A853', // Verde Google
    url: 'https://docs.google.com/spreadsheets/d/1_tx3ztvOFXFXHqqcL5NysbSy8AWQZilPEYwFB5hT4eY/preview?usp=sharing' 
  },
  { 
    id: 3, 
    texto: 'Acomodadores / Micrófonos', 
    color: '#EA4335', // Rojo Google
    url: 'https://docs.google.com/document/d/OTRO_DOC/preview' 
  },
   { 
    id: 4, 
    texto: 'Anuncios Importantes', 
    color: '#FBBC05', // Amarillo Google
    textoOscuro: true, // Para que la letra sea negra en fondo amarillo
    url: 'https://docs.google.com/document/d/OTRO_DOC_MAS/preview' 
  },
     { 
    id: 5, 
    texto: 'Servicio del Campo', 
    color: '#4285F4', // Azul Google
    textoOscuro: true, // Para que la letra sea negra en fondo amarillo
    url: 'https://docs.google.com/spreadsheets/d/1hCugqSpqz__vdScZk9ItBar0PLPUdbTDfSUeoTn__qo/preview?gid=0#gid=0' 
}
];

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
    
    <div v-if="!documentoActual" class="menu-vista">
      
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

    <div v-else class="documento-vista">
      <div class="barra-navegacion">
        <button @click="volverAlMenu" class="boton-volver">
          ← Regresar
        </button>
        <span class="titulo-doc">{{ tituloActual }}</span>
      </div>

      <iframe 
        :src="documentoActual" 
        class="visor-iframe" 
        frameborder="0" 
        allowfullscreen>
      </iframe>
    </div>

  </main>
</template>

<style scoped>
/* --- ESTILOS CSS --- */

.contenedor-principal {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  min-height: 100vh;
  background-color: #f8f9fa; /* Fondo gris muy claro */
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  /* Bordes redondeados para todo el contenedor en PC */
  border-radius: 16px; 
  overflow: hidden; /* Importante para que el banner respete las esquinas redondeadas */
}

/* ESTILOS DEL NUEVO BANNER */
.banner-container {
  width: 100%;
  height: 180px; /* Altura fija del banner. Ajústala si quieres. */
  background-color: #e0e0e0; /* Color mientras carga */
}

.banner-img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Esto hace que la imagen se recorte para llenar sin estirarse */
  object-position: center; /* Centra el foco de la imagen */
  display: block;
}

/* ESTILOS DEL ENCABEZADO (Ajustado) */
.encabezado {
  text-align: center;
  padding: 2rem 1rem 1rem 1rem; /* Menos padding abajo */
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

/* ESTILOS DE BOTONES COLORIDOS */
.lista-botones {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
  padding: 1.5rem;
  background-color: #fff; /* Fondo blanco para el área de botones */
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

/* VISOR DE DOCUMENTOS */
.documento-vista {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.barra-navegacion {
  background-color: #2c3e50;
  color: white;
  padding: 0.8rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  z-index: 10;
}

.boton-volver {
  background-color: #ffffff;
  color: #2c3e50;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 20px;
  font-weight: bold;
  cursor: pointer;
}

.titulo-doc {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 0.9rem;
}

.visor-iframe {
  width: 100%;
  flex-grow: 1;
  background: white;
}

.pie-pagina {
  text-align: center;
  color: #999;
  font-size: 0.9rem;
  padding: 1.5rem;
  background-color: #fff;
}
</style>