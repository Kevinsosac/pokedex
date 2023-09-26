<template>
  <div class="contenedor1">
    <div class="input-container">
      <input type="text" placeholder="Search Pokémon" v-model="searchTerm">
      <button class="button" @click="buscarPokemon">Search</button>
    </div>

  </div>
  <div class="contenedor2">
    <div v-for="(pokemon, index) in pokemones" :key="index" @click="mostrarModal(pokemon)">
      <div class="card">
        <div class="card">
          <div class="first-content">
            <img class="imagen-pokemon" :src="pokemon.img" alt="" />
          </div>
          <div class="second-content">
            <p class="text head">{{ pokemon.nombre }}</p>
            <p class="bg">ID:# {{ pokemon.numero }} </p>
            <p class="text price" v-for="(tipo, index) in pokemon.tipo_pk" :key="index">{{ tipo }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div v-if="info_modal" class="modal">
    <div class="modal-content">
      <button @click="ocultarModal" class="btn2">❌</button>
      <div class="container1">
        <header class="header2"></header>
        <div class="">
          <div class="row gx-0">
            <div class="col">
              <div class="p-5">
                <div class="pg-2">
                  <div class="primera-p">
                    <p class="info4">#{{ selectedPokemon.numero }}</p>
                  </div>

                  <div class="primera-p">
                    <label class="lb" for="">Height</label>
                    <p class="info">{{ selectedPokemon.altura }}</p>
                  </div>

                  <div class="primera-p">
                    <label class="lb" for="">Weight</label>
                    <p class="info">{{ selectedPokemon.peso }}</p>
                  </div>

                  <div class="primera-p">
                    <label class="lb" for="">Type</label>
                    <p class="ti">{{ selectedPokemon.tipo_pk.join(' / ') }}</p>
                  </div>
                </div>
              </div>
            </div>

            <div class="col">
              <div class="p-5">
                <h1 class="titulo2">{{ selectedPokemon.nombre }}</h1>
                <img :src="selectedPokemon.img" alt="" class="imagenPK" />
              </div>
            </div>

            <div class="col">
              <div class="p-5">
                <div class="pg-3">
                  <div class="contt">
                    <label for="HP">HP</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.hp" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.hp + '%' }">{{
                          selectedPokemon.hp
                        }}%
                      </div>
                    </div>
                  </div>

                  <div class="contt">
                    <label for="Attack">Attack</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.ataque" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.ataque + '%' }">{{
                          selectedPokemon.ataque }}%</div>
                    </div>
                  </div>

                  <div class="contt">
                    <label for="Defence">Defence</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.defensa" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.defensa + '%' }">{{
                          selectedPokemon.defensa }}%</div>
                    </div>
                  </div>

                  <div class="contt">
                    <label for="Sp. Attack">Sp. Attack</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.ataque_especial" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.ataque_especial + '%' }">{{ selectedPokemon.ataque_especial }}%
                      </div>
                    </div>
                  </div>

                  <div class="contt">
                    <label for="Sp. Defence">Sp. Defence</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.defensa_especial" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.defensa_especial + '%' }">{{ selectedPokemon.defensa_especial }}%
                      </div>
                    </div>
                  </div>

                  <div class="contt">
                    <label for="Speed">Speed</label>
                    <div class="progress" role="progressbar" aria-label="Animated striped example"
                      :aria-valuenow="selectedPokemon.velocidad" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated"
                        :style="{ width: selectedPokemon.velocidad + '%' }">
                        {{ selectedPokemon.velocidad }}%</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";


let pokemones = ref([]);
let info_modal = ref(false);
let selectedPokemon = ref(null);
let searchTerm = ref('');

async function obtenerUrlpokemon() {
  pokemones.value = [];

  for (let i = 1; i <= 50; i++) {
    let r = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}/`);
    let pokemon = {
      img: r.data.sprites.other["official-artwork"].front_default,
      numero: r.data.id,
      nombre: r.data.name,
      altura: r.data.height,
      peso: r.data.weight,
      tipo_pk: r.data.types.map((pk) => pk.type.name),
      hp: r.data.stats[0].base_stat,
      ataque: r.data.stats[1].base_stat,
      defensa: r.data.stats[2].base_stat,
      ataque_especial: r.data.stats[3].base_stat,
      defensa_especial: r.data.stats[4].base_stat,
      velocidad: r.data.stats[5].base_stat,
    };
    pokemones.value.push(pokemon);
  }
}

function mostrarModal(pokemon) {
  selectedPokemon.value = pokemon;
  info_modal.value = true;
}

function ocultarModal() {
  info_modal.value = false;
}

function buscarPokemon() {
  const term = searchTerm.trim().toLowerCase();

  if (term === '') {
    pokemonesFiltrados.value = [...pokemones.value];
  } else {
    pokemonesFiltrados.value = pokemones.value.filter(pokemon => {
      return pokemon.nombre.toLowerCase().includes(term);
    });
  }
}


obtenerUrlpokemon();
</script>

<style scoped>
.contenedor1 {
  background-size: cover;
  object-fit: cover;
  height: 22vh;
  background-position: center;
}

.lb {
  color: #ed2c1e;
  font-size: 35px;
}

.bg {
  font-size: 12px;
  color: lightgrey;
}

.header1 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 56px;
  display: flex;
  align-items: center;
  background-color: #00000059;
}

.contenedor2 {
  display: grid;
  background-repeat: no-repeat;
  background-position: center;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  text-align: center;
  width: 99%;
  margin-left: 5px;
  gap: 10px;
}

.tarjet {
  cursor: pointer;
}

select {
  background-color: transparent;
  border: 0;
}

.contenedor2>div {
  padding: 20px;
}

.titulo1 {
  text-align: center;
}

.input-container {
  width: 33%;
  position: absolute;
  left: 76%;
  top: 10%;
  transform: translate(-50%, -50%);
}

.imagen-pokemon {
  width: 193px;
  height: 190px;
  margin-top: 13%;
}

.info2 {
  display: flex;
  gap: 15px;
}

.btn2 {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: transparent;
  border: 0;
  cursor: pointer;
}

.titulo2 {
  position: relative;
  left: 20px;
  bottom: 50px;
  margin-top: 10%;
  background-color: #4f46468a;
  border-radius: 60px;
}

.pg-2 {
  margin-top: 20%;
  perspective: 300px;
  transition: .3s;
}

.input-group {
  display: flex;
  align-items: center;
}

.input__container--variant {
  background: linear-gradient(to bottom, #F3FFF9, #F3FFF9);
  border-radius: 30px;
  max-width: 13em;
  padding: 1em 1em;
}

.shadow__input--variant {
  filter: blur(25px);
  border-radius: 30px;
  background-color: #F3FFF9;
  opacity: 0.5;
}

.input__button__shadow--variant {
  border-radius: 15px;
  background-color: #ffffff;
  padding: 10px;
  border: none;
}

.input__button__shadow--variant:hover {
  background-color: #3C6659;
}

.input__search--variant {
  width: 13em;
  align-items: center;
  border-radius: 13em;
  outline: none;
  border: none;
  padding: 0.8em;
  font-size: 1.2em;
  color: #002019;
  background-color: transparent;
}

.input__search--variant::placeholder {
  color: #002019;
  opacity: 0.7;
}

.input__container--variant {
  background: linear-gradient(to bottom, #F3FFF9, #F3FFF9);
  border-radius: 2.5em;
  max-width: 18em;
  padding: 1em;
  box-shadow: 0em 1em 3em #beecdc64;
}






.pg-3 {
  position: relative;
  top: 25px;
  width: 100%;
  perspective: 300px;
  transition: .3s;
}

.primera-p {
  display: flex;
  justify-content: space-between;
  margin: 25px;
}

label {
  font-weight: 700;
  color: #222121;
  font-size: 15px;
}

.imagenPK {
  position: relative;
  width: 200%;
  right: 95px;
  bottom: 56px;
  background-size: cover;
  height: 344px;
}


.contt {
  display: flex;
  justify-content: flex-end;
  width: 400px;
  align-items: center;
}

.info4 {
  font-size: 95px;
  margin-right: 25%;
}

.progress-bar {
  text-align: right;
  background-color: #ff0000;
  border-radius: 4px;
  transition: background-color 0.4s ease;
}

.progress {
  border: 1px solid black;
  background-color: rgb(0, 0, 0);
  margin: 15px;
  margin-left: 35px;
  width: 200px;
}


.card {
  width: 200px;
  height: 284px;
  background: rgb(250, 254, 255);
  transition: all 0.4s;
  border-radius: 10px;
  box-shadow: 0px 0px 10px 5px rgba(46, 44, 44, 0.705);
  font-size: 30px;
  font-weight: 900;
}

.card:hover {
  border-radius: 15px;
  cursor: pointer;
  transform: scale(1.2);
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.705);
  background: rgb(187, 135, 135);
}

.first-content {
  height: 100%;
  width: 100%;
  transition: all 0.4s;
  justify-content: center;
  align-items: center;
  opacity: 1;
  border-radius: 15px;
}

.card:hover .first-content {
  height: 0px;
  opacity: 0;
}

.second-content {
  height: 100%;
  width: 100%;
  opacity: 0;
  justify-content: center;
  align-items: center;
  border-radius: 15px;
  transition: all 0.4s;
  font-size: 0px;
  transform: rotate(90deg) scale(-1);
  margin-top: 20%;
}

.card:hover .second-content {
  opacity: 1;
  height: 100%;
  font-size: 1.8rem;
  transform: rotate(0deg);
}

.modal {
  position: fixed;
  display: flex;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal-content {
  background-color: #ffffff;
  width: 90%;
  height: 90%;
  overflow-y: auto;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

.col {
  padding: 20px;
}

/* Estilos para la información de Pokémon en la primera columna */
.primera-p {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.info {
  color: #333;
  font-size: 85px;
}

/* Estilos para el nombre del Pokémon en la segunda columna */
.titulo2 {
  font-size: 24px;
  color: #333;
  margin-top: 10px;
}

/* Estilos para la imagen del Pokémon en la segunda columna */
.imagenPK {
  width: 147%;
  height: 646px;
  object-fit: contain;
  margin-top: 21%;
}

/* Estilos para las estadísticas en la tercera columna */
.pg-3 {
  margin-top: 20px;
  perspective: 300px;
  transition: 0.3s;
}

.contt {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

label {
  font-weight: 700;
  color: #222121;
  font-size: 15px;
}

.progress {
  background-color: #000000;
  border-radius: 4px;
}

.progress-bar {
  text-align: right;
  border-radius: 4px;
  transition: background-color 0.4s ease;
}

/* Estilos para el botón de cierre del modal */
.btn2 {
  position: absolute;
  top: 100px;
  right: 10px;
  background-color: transparent;
  border: 0;
  cursor: pointer;
  font-size: 20px;
  color: #00ff48;
}

.btn2:hover {
  color: #ff0000;
}

@media screen and (max-width: 1200px) {

  .pg-2,
  .pg-3 {
    transform: none;
  }


  .pg-3 {
    margin-top: 0;
    position: relative;
    right: 105px;
  }

  .imagenPK {
    position: relative;
    width: 300px;
    height: 350px;
    background: b;
  }
}

@media screen and (max-width: 380px) {
  .modal-content {
    max-width: 100%;
  }
}</style>
