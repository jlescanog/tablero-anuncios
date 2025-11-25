<script setup>
// IMPORTAMOS LA UTILIDAD PARA CREAR VARIABLES REACTIVAS
import { ref } from 'vue';

// --- ESTADO DE LA APLICACIÓN ---
// 'documentoActual': Guarda el link del documento que se está viendo. Si es null, muestra el menú.
// 'tituloActual': Guarda el nombre del botón que se presionó para mostrarlo arriba.
const documentoActual = ref(null);
const tituloActual = ref('');

// --- CONFIGURACIÓN DE BOTONES ---
// Aquí es donde cambiarás los links en el futuro.
// IMPORTANTE: Asegúrate de que los links terminen en '/preview' para que se vean bien en celulares.
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

// Función para abrir el documento cuando tocan un botón
const abrirDocumento = (item) => {
  tituloActual.value = item.texto;
  documentoActual.value = item.url;
  window.scrollTo(0, 0); // Sube la pantalla al inicio por si estaban abajo
};

// Función para el botón "Regresar"
const volverAlMenu = () => {
  documentoActual.value = null; // Al poner null, la web sabe que debe mostrar el menú de nuevo
  tituloActual.value = '';
};
</script>

<template>
  <main class="contenedor-principal">
    
    <div v-if="!documentoActual" class="menu-vista">
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
/* --- ESTILOS VISUALES (CSS) --- */

/* Estilo base de toda la app */
.contenedor-principal {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 600px; /* Ancho máximo similar a un celular grande */
  margin: 0 auto;   /* Centrado en pantalla de PC */
  min-height: 100vh;
  background-color: #f0f2f5;
  box-shadow: 0 0 20px rgba(0,0,0,0.1); /* Sombra suave a los lados en PC */
}

/* Estilos del Encabezado */
.encabezado {
  text-align: center;
  padding: 2.5rem 1rem;
  background-color: #ffffff;
  border-bottom: 1px solid #e0e0e0;
}

.encabezado h1 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.8rem;
  font-weight: 800;
  text-transform: uppercase;
}

/* Estilos de la lista de botones */
.lista-botones {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
  padding: 1.5rem;
}

.boton-anuncio {
  border: none;
  padding: 1.5rem;
  font-size: 1.3rem; /* Letra grande para lectura fácil */
  font-weight: bold;
  border-radius: 12px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0,0,0,0.15);
  transition: transform 0.1s, filter 0.2s;
  text-align: center;
  width: 100%;
}

.boton-anuncio:active {
  transform: scale(0.97); /* Efecto de "presionar" */
}

/* Estilos de la vista de documento */
.documento-vista {
  display: flex;
  flex-direction: column;
  height: 100vh; /* Ocupa toda la altura de la pantalla */
}

.barra-navegacion {
  background-color: #2c3e50;
  color: white;
  padding: 0.8rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  z-index: 10;
}

.boton-volver {
  background-color: #ffffff;
  color: #2c3e50;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 20px;
  font-weight: bold;
  font-size: 1rem;
  cursor: pointer;
}

.titulo-doc {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 0.9rem;
  opacity: 0.9;
}

/* El visor del documento */
.visor-iframe {
  width: 100%;
  flex-grow: 1; /* Hace que ocupe todo el espacio sobrante */
  background: white;
}

.pie-pagina {
  text-align: center;
  color: #888;
  font-size: 0.9rem;
  padding-bottom: 2rem;
}
</style>