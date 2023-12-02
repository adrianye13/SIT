<template>
    <body class="bg-gradient-to-r from-cyan-300 to-green-300 w-full h-full">
      <!-- Barra de navegación -->
      <ul class=" flex items-center justify-center bg-gradient-to-r from-green-700 to-cyan-700 space-x-4 h-16">
        <li>
          <div class="flex space-x-4">
            <h1 class="text-center text-white text-5xl font-['Chiller'] font-bold">INSTITUTO TECNOLOGICO SUPERIOR DE MOTUL</h1>
          </div>
        </li>
      </ul>
  
      <img src="/sit.png" alt="" class="mx-auto mt-4" style="max-width: 150%; max-height: 150px;" />
  
      <!-- Contenedor principal -->
      <div class="flex flex-row items-center justify-center">
        <div v-if="isLoading" class="loading">Cargando...</div>
        <div v-else>
          <!-- Contenido -->
          <div class="w-full p-1">
            <!-- Primer bloque -->
       
              <div class="text-3xl text-center font-['centaur'] font-bold mb-4 text-blue-600">
                {{ customTitles.periodo }}
              </div>
              <div class="text-xl text-center mb-4 text-gray-800">{{ info.periodo }}</div>
           
            <br>
  
            <div class="flex space-x-4">
  <!-- Primer bloque -->
  <div class="bg-white rounded-lg shadow-md p-6 transition-transform transform hover:scale-105">
    <div class="text-lg font-bold mb-4 text-blue-600">
      {{ customTitles.inicio }}
    </div>
    <div class="text-xl mb-4 text-gray-800">{{ formatDate(info.inicio) }}</div>
  </div>

  <!-- Segundo bloque -->
  <div class="bg-white rounded-lg shadow-md p-6 transition-transform transform hover:scale-105">
    <div class="text-lg font-bold mb-4 text-blue-600">
      {{ customTitles.fin }}
    </div>
    <div class="text-xl mb-4 text-gray-800">{{ formatDate(info.fin) }}</div>
  </div>

  <!-- Tercer bloque -->
  <div class="bg-white rounded-lg shadow-md p-6 transition-transform transform hover:scale-105">
    <div class="text-lg font-bold mb-4 text-blue-600">
      <p>Horas restantes</p>
    </div>
    <div class="text-xl mb-4 text-gray-800">{{ calculateHoursRemaining(info.fin) }}</div>
  </div>
</div>

         
<br>

            </div>
<br>
              <div class="flex space-x-4">
              <!-- FLEX 1 bloque -->
              <div class="bg-white rounded-full shadow-md p-4 transition-transform transform hover:scale-105 w-52 h-52">
                <div class="text-lg font-bold mb-4 text-center text-blue-600">
                  {{ customTitles.ISC }} 
                     <img src="/codificacion.png" alt="" class="mx-auto mt-4" style="max-width: 52%; max-height: 52px;" />
                </div>
                <div class="text-0xl mb-4 text-center text-gray-800">{{ formatValue (info.ISC) }}</div>
              </div>
              <br>
  
              <!-- FLEX 2 bloque -->
              <div class="bg-white rounded-full shadow-md p-6 transition-transform transform hover:scale-105 w-52 h-52">
                <div class="text-lg font-bold text-center mb-4 text-blue-600">
                  {{ customTitles.II }}
                </div>
                <div class="text-xl mb-4 text-center text-gray-800">{{ formatValue (info.II) }}</div>
              </div>

                            <!-- FLEX 3 bloque -->
                            <div class="bg-white rounded-full shadow-md p-6 transition-transform transform hover:scale-105 w-52 h-52">
                <div class="text-lg font-bold text-center mb-4 text-blue-600">
                  {{ customTitles.IEM }}
                </div>
                <div class="text-xl mb-4 text-center text-gray-800">{{ formatValue (info.IEM) }}</div>
              </div>
            
    <!-- FLEX 4 bloque -->
    <div class="bg-white rounded-full shadow-md p-6 transition-transform transform hover:scale-105 w-52 h-52"
    @mouseover="showTooltip4 = true"
    @mouseleave="showTooltip4 = false">
    <div class="text-lg text-center font-bold mb-4 text-blue-600">
      {{ customTitles.IE }}
      <img src="/codificacion.png" alt="" class="mx-auto mt-4" style="max-width: 52%; max-height: 52px;" />
    </div>
    <div class="text-xl text-center mb-4 text-gray-800" :title="showTooltip4 ? info.IER : ''">
      {{ showTooltip ? formatValue(info.IE) : '' }}
    </div>
  </div>
            
   <!-- FLEX 5 bloque -->
              <div class="bg-white rounded-full shadow-md p-6 transition-transform transform hover:scale-105 w-52 h-52"
    @mouseover="showTooltip = true"
    @mouseleave="showTooltip = false">
    <div class="text-lg text-center font-bold mb-4 text-blue-600">
      {{ customTitles.IER }}
    </div>
    <div class="text-xl text-center mb-4 text-gray-800" :title="showTooltip ? info.IER : ''">
      {{ showTooltip ? formatValue(info.IER) : '' }}
    </div>
  </div>

            <!-- Repite la estructura para otros campos si es necesario -->
          </div>
        </div>
      </div>
      <br>
      <br>
      <br>
      <br>
      <br>
    </body>
  </template>
  
  <script setup>
  import { onMounted, ref } from 'vue';

  const showTooltip = ref(false)
  const showTooltip4 = ref(false)
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
    ISC: 'ISC',
    II: 'II',
    IEM: 'IEM',
    IER: 'IER',
    IE: 'IE'
  };
  
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
  
      isLoading.value = false;
    } catch (error) {
      console.error('Error al obtener datos:', error);
    }
  }

  function formatValue(value) {
  if (typeof value === 'object') {
    const sum = value.faltantes + value.listas;
    const remaining = sum - value.listas;
    return `${value.listas} de ${sum} completadas, ${remaining} restantes`;
  } else {
    return value;
  }
}

function calculateHoursRemaining(deadline) {
	const fechaLimite = new Date(deadline);
	const ahora = new Date();

	// Calcula la diferencia en horas
	const diferenciaHoras = Math.floor((fechaLimite - ahora) / (1000 * 60 * 60));

	return `${diferenciaHoras} horas`;
}

function formatDate(dateString) {
  // Convierte la cadena de fecha a objeto Date
  const dateObject = new Date(dateString);

  // Obtiene el año, mes y día
  const year = dateObject.getFullYear();
  const month = (dateObject.getMonth() + 1).toString().padStart(2, '0'); // Añade cero al principio si es necesario
  const day = dateObject.getDate().toString().padStart(2, '0'); // Añade cero al principio si es necesario

  // Formatea la fecha como "YYYY-MM-DD"
  const formattedDate = `${year}-${month}-${day}`;

  return formattedDate;
}

  onMounted(() => {
    fetchData();
  });
  </script>
  

  
  