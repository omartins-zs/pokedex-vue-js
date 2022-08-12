<template>
  <v-app>
    <v-container>
      <v-text-field v-model="search" label="Pesquisar" placeholder="Placeholder" solo>
      </v-text-field>
      <v-card>
        <v-container>
          <!-- {{ pokemons }} -->
          <v-row class="mx-0 d-flex justify-center">
            <v-col cols="3" v-for="pokemon in filtered_pokemons" :key="pokemon.name">
              <v-card @click="show_dialog = !show_dialog">
                <v-container>
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(pokemon)}.png`"
                    :alt="pokemon.name" srcset="" width="80%">
                  <h2 class="text-center">{{ pokemon.name }}</h2>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
    </v-container>
    <div class="text-center">
     
      <v-dialog v-model="show_dialog" width="800">
          <v-container>
        <v-card @click="show_pokemon(get_id(pokemon))">
          <v-card-title class="text-h5 grey lighten-2">
            Privacy Policy
          </v-card-title>

          <v-card-text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et
            dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex
            ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat
            nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit
            anim id est laborum.
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">
              I accept
            </v-btn>
          </v-card-actions>
          </v-container>
        </v-card>
      </v-dialog>
    </div>
  </v-app>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',

  components: {

  },

  data: () => {
    return {
      pokemons: [],
      search: "",
      show_dialog: false,
      selected_pokemon: null
    }
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=150")
      .then((response) => {
        this.pokemons = response.data.results;
        // console.log(response);
      })
  },

  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    show_pokemon(id){
      axios.get("https://pokeapi.co/api/v2/pokemon/${id}")
      .then((response) => {
        this.selected_pokemon = response.data;
        // console.log(response);
      this.show_dialog = !this.show_dialog
      })
    }
  },
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      })
    },
  },
};
</script>
<style>
#app {
  background: linear-gradient(to bottom right,
      rgba(10, 10, 10, 1),
      rgba(12, 39, 63, 1)) no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>