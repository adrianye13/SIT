<template>
   <body class= "bg-[#E6E2E0]">
  <div>
<!-- Modificado para centrar horizontalmente -->
    <div v-if="isLoading" class="loading">Cargando...</div>
    <div v-else>
      <div class="text-center text-7-l"> 
        <h1 class="font-bold">DATOS DE EVALUACIÓN A TUTOR</h1>
      </div>
     
      <div class="status-container  container-center"> <!-- Modificado para centrar verticalmente -->
        <div v-for="(value, key, index) in info" :key="key" :class="[getCardClass(index), getCardSizeClass(index)]">
          <div :class="getTitleClass(index)">{{ customTitles[key] || key }}</div>
          <div :class="getValueClass(index)" v-if="typeof value !== 'object'">{{ value }}</div>
        </div>
      </div>
    </div>
  </div>
  
  </body>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const info = ref('');
const isLoading = ref(true);
const customTitles = {
  periodo: 'Periodo',
  inicio: 'Inicio de evaluación',
  fin: 'Fin de evaluación',
  alTotal: 'Total de alumnos',
  alEvaluados: 'Total de evaluaciones',
  alListas: 'Evaluaciones Listas',
  alFaltantes: 'Evaluaciones Faltantes',
};

// Array de colores para cada recuadro
const cardColors = ['bg-lime-950', 'bg-green-300', 'bg-green-500', 'bg-yellow-500', 'bg-purple-500'];

async function fetchData() {
  try {
    const response1 = await fetch('https://sitmotul.com.mx/api/statusEval');
    const data1 = await response1.json();

    const response2 = await fetch('https://sitmotul.com.mx/api/statusEvalIng');
    const data2 = await response2.json();

    // Combina los datos de ambas API
    info.value = {
      ...data1,
      ...data2,
    };

    // Cambia los nombres de las claves según el mapeo personalizado
    info.value = mapKeys(info.value, customTitles);

    isLoading.value = false;
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}

// Función para mapear claves de un objeto según un mapa personalizado
function mapKeys(obj, keyMap) {
  return Object.fromEntries(
    Object.entries(obj).map(([key, value]) => [
      keyMap[key] || key,
      formatValue(value),
    ])
  );
}

// Función para formatear el valor según tus requisitos
function formatValue(value) {
  if (typeof value === 'object') {
    // Si el valor es un objeto, asumimos que es un formato específico
    // Puedes ajustar esta lógica según la estructura real de tu objeto
    return `${value.listas} de ${value.faltantes} completadas`;
  } else {
    // Si el valor no es un objeto, lo dejamos sin cambios
    return value;
  }
}

// Función para obtener la clase de color para cada recuadro
function getCardClass(index) {
  return `rounded-lg shadow-md m-4 p-8 ${cardColors[index % cardColors.length]}`;
}

// Función para obtener la clase de tamaño para cada recuadro
function getCardSizeClass(index) {
  // Define tus clases de tamaño personalizadas y aplícalas según el índice
  const sizeClasses = ['w-full', 'w-64', 'w-64', 'w-64', 'w-64', 'w-64', 'w-64', 'w-64', 'w-64', 'w-64']; // Puedes ajustar los tamaños según tus necesidades
  return sizeClasses[index % sizeClasses.length];
}

// Función para obtener la clase de estilo de texto para los títulos
function getTitleClass(index) {
  // Define tus clases de estilo de texto para los títulos y aplícalas según el índice
  const titleClasses = ['text-lg text-center font-bold text-white', 'text-lg text-center font-bold text-black', 'text-lg text-center font-bold text-black', 'text-lg text-center font-bold text-black','text-lg text-center font-bold text-black', 'text-lg text-center font-bold text-black', 'text-lg text-center font-bold text-black']; // Puedes ajustar los estilos según tus necesidades
  return titleClasses[index % titleClasses.length];
}

// Función para obtener la clase de estilo de texto para los valores
function getValueClass(index) {
  // Define tus clases de estilo de texto para los valores y aplícalas según el índice
  const valueClasses = ['text-base text-center text-green-500', 'text-center text-lg text-black', 'text-lg text-purple-500', 'text-base text-yellow-500', 'text-base text-red-500']; // Puedes ajustar los estilos según tus necesidades
  return valueClasses[index % valueClasses.length];
}

onMounted(() => {
  fetchData();
});
</script>
