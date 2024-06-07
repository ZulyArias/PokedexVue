<!-- Consola de buscar el pokemon -->

<template>
  <div id="app">
    <header>
      <div class="logoPrincipal">
        <img src="https://archives.bulbagarden.net/media/upload/4/4b/Pok%C3%A9dex_logo.png" alt="Pokédex">
      </div>
      <div class="busquedaInput">
        <input class="ingresado" type="text" placeholder="Buscar nombre de pokemon" v-model="search">
        <button @click="traerDatos()"><i class="fa-brands fa-searchengin"></i></button>
      </div>
    </header>
    <main v-if="nombrePoke">
      <div class="infoPokemon">
        <div class="infoImg">
          <img :src="imagenPoke">
        </div>

        <div class="infoNombre">
          <h2>Nombre: {{ nombrePoke }}</h2>
        </div>

        <div class="infoId">
          <p> # {{ idPoke }}</p>
        </div>

        <div class="infoTipo">
          <h3> Tipo: </h3>
          <ul>
            <li v-for="tipo in tipoPoke" :key="tipo" class="typePoke">
              {{ tipo }}
            </li>
          </ul>
        </div>

        <div class="infoMedidas">
          <div class="peso">
            <p>Peso: {{ pesoPoke / 10 }} kg⚖️</p>
          </div>
          <div class="altura">
            <p>Altura: {{ alturaPoke / 10 }} m📏</p>
          </div>
        </div>

        <div class="estadisticas">
          <h3>Estadísticas:</h3>
          <ul>
            <li v-for="stat in estadisticasPoke" :key="stat.name">
              <span>{{ statEmojis[stat.name] }} {{ stat.name }}:</span>
              <div class="stat-bar">
                <div class="progress" :style="{ width: (stat.base_stat / 255 * 100) + '%' }"> {{ stat.base_stat }} / 255
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </main>
    <div v-else class="loading">
      <p>Buscando Pokémon...</p>
    </div>
  </div>
</template>





<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'

// variables
const search = ref('')
const imagenPoke = ref('')
const nombrePoke = ref('')
const idPoke = ref('')
const tipoPoke = ref([])
const pesoPoke = ref(0)
const alturaPoke = ref(0)
const estadisticasPoke = ref([])
const fondoColor = ref('')

// defino los emoijs para las estadísticas
const statEmojis = {
  hp: '❤️',
  attack: '⚔️',
  defense: '🛡️',
  'special-attack': '💥☠️',
  'special-defense': '🛡️✨',
  speed: '🏃💨'
}



// función para traer los datos de la página:
const traerDatos = async () => {
  if (search.value) {
    try {
      const res = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${search.value}`
      );

      imagenPoke.value = res.data.sprites.other["official-artwork"].front_default;
      nombrePoke.value = res.data.name;
      idPoke.value = res.data.id;
      tipoPoke.value = res.data.types.map(element => element.type.name)
      pesoPoke.value = res.data.weight;
      alturaPoke.value = res.data.height;
      estadisticasPoke.value = res.data.stats.map((stat) => {
        return {
          name: stat.stat.name,
          base_stat: stat.base_stat
        }
      })

      cambiarColorFondo(tipoPoke.value);
    } catch (error) {
      console.error("Error al obtener los datos:", error);
    }
  }
}

// función para cambiar el color de fondo
const cambiarColorFondo = (tipo) => {
  switch (tipo[0]) {
    case "fire":
      fondoColor.value = "red";
      break;
    case "water":
      fondoColor.value = "blue";
      break;
    case "grass":
      fondoColor.value = "green";
      break;
    case "electric":
      fondoColor.value = "yellow";
      break;
    case "psychic":
      fondoColor.value = "purple";
      break;
    case "ice":
      fondoColor.value = "lightblue";
      break;
    case "dragon":
      fondoColor.value = "darkred";
      break;
    case "dark":
      fondoColor.value = "#36454F";
      break;
    case "fairy":
      fondoColor.value = "pink";
      break;
    case "normal":
      fondoColor.value = "gray";
      break;
    case "fighting":
      fondoColor.value = "brown";
      break;
    case "flying":
      fondoColor.value = "skyblue";
      break;
    case "poison":
      fondoColor.value = "#4B0082";
      break;
    case "ground":
      fondoColor.value = "#8B4513";
      break;
    case "bug":
      fondoColor.value = "#6B8E23";
      break;
    case "rock":
      fondoColor.value = "#696969";
      break;
    case "ghost":
      fondoColor.value = "#6A5ACD";
      break;
    case "steel":
      fondoColor.value = "#B0C4DE";
      break;
    default:
      fondoColor.value = "";
      break;
  }
}

// funcion para hacer los cambios del fondo visibles
watch(tipoPoke, (newValue) => {
  cambiarColorFondo(newValue);
  document.documentElement.style.setProperty('--fondoColor', fondoColor.value);
});

</script>




<style>
body {
  font-family: "Press Start 2P", system-ui;
}

header {
  position: sticky;
  top: 0;
  z-index: 1;
  /* background-color: azure; */
  background-image: url(https://i.pinimg.com/736x/c7/2a/ee/c72aee7ba40af4513b7cf301cb4b4d81.jpg);
  padding: 20px;
  filter: drop-shadow(8px 8px 10px rgba(0, 0, 0, 0.63));
}

img {
  filter: drop-shadow(8px 8px 10px rgba(0, 0, 0, 0.63));
}

.logoPrincipal {
  display: flex;
  justify-content: center;
  align-items: center;
}

header input {
  width: 100%;
  padding: 10px;
  border-radius: 10px;
}

.ingresado {
  border: 1ex solid none;
  border-top-width: 1.7em;
  margin: 0;
  padding: 0;
  color: azure;
  word-wrap: break-word;
  outline: 6px solid #3f51b5;
  height: 30px;
  font-size: 17px;
  text-align: center;
  transition: all 1s;
  font-weight: bold;
  font-family: 'Courier New', Courier, monospace;
}

.ingresado:hover {
  border-top-width: 0.2em;
  background-color: #f1e8e8;
  outline: 6px solid rgb(255, 208, 0);
}

.busquedaInput {
  display: flex;
  align-items: center;
  justify-content: center;
  justify-items: center;
  align-content: center;
}

.busquedaInput button {
  background-color: #3f51b5;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 10px;
  cursor: pointer;
  height: 100%;
  width: 10%;
  position: relative;
  transition: 0.5s ease;
  user-select: none;
}

.busquedaInput button:hover,
:focus {
  color: #3f51b5;
  background: rgb(255, 208, 0);
  border: 1px solid rgb(255, 208, 0);
  text-shadow: 0 0 5px #ffffff, 0 0 10px #ffffff, 0 0 20px #ffffff;
  box-shadow: 0 0 5px rgb(255, 208, 0), 0 0 20px rgb(255, 208, 0), 0 0 50px rgb(255, 208, 0),
    0 0 100px rgb(255, 208, 0);
}

main {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 20px;
  background-color: var(--fondoColor);
}

main div {
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 2px 5px var(--fondoColor);
}

.infoImg img {
  width: 100%;
  height: auto;
  max-height: 300px;
  object-fit: contain;
}

.infoNombre h2 {
  margin-top: 0;
  text-align: center;
}

.infoId p {
  text-align: center;
  margin-bottom: 0;
}

.infoTipo {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 1rem;
}

.infoTipo ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  gap: 0.5rem;
}

.infoTipo li {
  width: fit-content;
  padding: 0.8em 1.1em;
  gap: 0.4rem;
  border: none;
  font-weight: bold;
  border-radius: 30px;
  cursor: pointer;
  text-shadow: 2px 2px 3px rgb(136 0 136 / 50%);
  background: linear-gradient(15deg,
      #880088,
      #aa2068,
      #cc3f47,
      #de6f3d,
      #f09f33,
      #de6f3d,
      #cc3f47,
      #aa2068,
      #880088) no-repeat;
  background-size: 300%;
  background-position: left center;
  transition: background 0.3s ease;
  color: #fff;
}

.infoTipo li:hover {
  background-size: 320%;
  background-position: right center;
}

.infoTipo li:hover svg {
  fill: #fff;
}

.infoTipo li svg {
  width: 23px;
  fill: #f09f33;
  transition: 0.3s ease;
}

.infoMedidas {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-top: 1rem;
  text-align: center;
}

.peso, .altura {
  background: linear-gradient(135deg, #33f0f0, #3dbede);
  padding: 1rem 2rem;
  border-radius: 10px;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.1);
  color: white;
  font-weight: bold;
  transition: transform 0.3s ease, background 0.3s ease;
  justify-content: center;
  align-items: center;
  align-content: center;
  justify-items: center;
}

.peso:hover, .altura:hover {
  transform: scale(1.05);
  background: linear-gradient(135deg, #3dbede, #33f0f0);
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.384);
}

.estadisticas {
  text-align: center;
}

.estadisticas ul {
  list-style: none;
  padding: 0;
}

.stat-bar {
  background-color: #eee;
  border-radius: 5px;
  overflow: hidden;
  height: 30%;
  margin-top: 5px;
}

.progress {
  background-color: var(--fondoColor);
  height: 100%;
  width: 0;
  animation: progress-fill 1s ease-in-out forwards;
}

@keyframes progress-fill {
  from {
    width: 0%;
  }
}

.stat-bar .progress {
  width: calc((var(--base-stat) / 255) * 100%);
}

.loading{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  position: absolute;
  top: 50%; 
}

@media screen and (max-width: 768px) {

  .busquedaInput {
    flex-direction: column;
    gap: 1rem;
  }

  .busquedaInput button {
    width: 100%;
  }

  .infoPokemon {
    width: 100%;
  }

  .infoImg img {
    max-height: 200px;
  }

  .infoTipo {
    flex-wrap: wrap;
  }

  .infoTipo li {
    width: 100%;
  }

  .infoMedidas {
    flex-direction: column;
    gap: 1rem;
  }

  .peso, .altura {
    padding: 0.5rem 1rem;
  }

  .estadisticas {
    width: 100%;
  }

  .stat-bar {
    height: 20%;
  }

  .stat-bar .progress {
    height: 100%;
  }

  .stat-bar .progress {
    width: calc((var(--base-stat) / 255) * 100%);
  }
}

/* estilos para pantallas menores a 685px, ajuste de tamaño de letra para distribuir bien el contenido */
@media screen and (max-width: 685px) {
  .ingresado {
    font-size: 15px;
  }

  .infoNombre h2 {
    font-size: 20px;
  }

  .infoId p {
    font-size: 15px;
  }

  .infoTipo li {
    padding: 0.5em 0.7em;
  }

  .infoMedidas {
    gap: 1rem;
  }

  .peso, .altura {
    padding: 0.5rem 1rem;
  }

  .estadisticas {
    font-size: 15px;
  }

  .estadisticas h3{
    font-size: 20px;
  }

  .stat-bar {
    font-size: 10px;
  }
  .stat-bar {
    height: 20%;
  }

  .stat-bar .progress {
    height: 100%;
  }

  .stat-bar .progress {
    width: calc((var(--base-stat) / 255) * 100%);
  }
}

</style>