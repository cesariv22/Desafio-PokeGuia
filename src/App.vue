<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <img
            src="./assets/logoPokemon.png"
            alt="logo pokemon"
            class="logo-poke"
          />
        </div>
        <div class="col-12">
          <h1>PokeGuía</h1>
        </div>
        <div class="col-12">
          <div class="col-12 d-flex justify-content-center">
            <label class="my-auto me-2">Nombre</label>
            <input type="text" v-model="pokeInput" />
            <button
              class="btn btn-primary ms-2"
              type="button"
              @click="buscarPoke"
            >
              Buscar
            </button>
          </div>
          <div class="col-12 d-flex justify-content-center mt-5">
            <div>
              <img class="img-poke" :src="imagenPoke" :alt="pokemonData.name" />
              <div>
                <h3>{{ tituloMovimientos }}</h3>
                <p v-for="movimiento in movimientos" :key="movimiento.name">
                  {{ movimiento.move.name }}
                </p>
                <h3>{{ tituloHabilidades }}</h3>
                <p v-for="habilidad in habilidades" :key="habilidad.name">
                  {{ habilidad.ability.name }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      URL: "https://pokeapi.co/api/v2/pokemon/",
      pokeInput: "pikachu",
      pokemonData: {},
    };
  },

  computed: {
    tituloMovimientos() {
      const tituloMovimientos = "movimientos";
      return tituloMovimientos.toUpperCase();
    },
    tituloHabilidades() {
      const tituloHabilidades = "habilidades";
      return tituloHabilidades.toUpperCase();
    },
    movimientos() {
      return this.pokemonData?.moves;
    },
    habilidades() {
      return this.pokemonData?.abilities;
    },
    imagenPoke() {
      return this.pokemonData?.sprites?.other.dream_world.front_default;
    },
  },
  created() {
    this.getPokemon();
  },

  methods: {
    async getPokemon() {
      try {
        const respuesta = await fetch(this.URL + this.pokeInput);
        const data = await respuesta.json();
        this.pokemonData = data;
      } catch (error) {
        console.error(error);
      }
    },
    buscarPoke() {
      this.getPokemon();
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.logo-poke {
  width: 180px;
}
.img-poke {
  width: 150px;
}
</style>
