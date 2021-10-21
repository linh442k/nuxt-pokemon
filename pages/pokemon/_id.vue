<template>
  <div class="pokemon-info">
    <div v-if="$fetchState.pending">
      Loading...
    </div>

    <div v-else-if="$fetchState.error">
      An error occurred :(
    </div>
    <div v-else>
      <div v-if="pokemonInfo" class="mt-6">
        <h1>{{ pokemonInfo.name }}</h1>
        <v-img
          max-height="120"
          max-width="120"
          :src="{src: pokemonInfo.sprites.front_default}"
        >
          <template #placeholder>
            Loading...
          </template>
        </v-img>
        <div class="stats d-flex flex-wrap text-center">
          <div v-for="(stat, index) in pokemonInfo.stats"
               :key="`stat-${index}`"
               class="pa-6 ma-3">
            <v-responsive width="12rem">
              <h2>{{ stat.stat.name }}</h2>
              <v-progress-circular
                :rotate="rotate[Math.floor(Math.random()*rotate.length)]"
                :size="100"
                :width="15"
                :value="statPercent(stat.base_stat)"
                :color="color[index]"
              >
                {{ stat.base_stat }}
              </v-progress-circular>
            </v-responsive>
          </div>
        </div>
      </div>

      <div v-else>
        Invalid Pokemon
      </div>
    </div>
    <v-btn
      class="mr-4 mt-6"
      :to="{name: 'index'}"
    >
      Back to Home
    </v-btn>
  </div>
</template>
<script>
import axios from "@nuxtjs/axios";

export default {
  data() {
    return {
      color: ["teal", "red", "blue", "orange", "cyan", "purple"],
      rotate: [-90, 90, 180, 360],
      pokemonInfo: null
    };
  },
  computed: {
    pokemonApi: function () {
      return "https://pokeapi.co/api/v2/pokemon/" + this.$route.params.id;
    }
  },
  watch: {
    "$route": "fetchData"
  },
  async fetch() {
   await this.$axios.$get(this.pokemonApi).then(
    response => {
       this.pokemonInfo=response;
       // console.log(response);
     }
   ).catch(error=>{
     console.log(error);
   });

  },
  methods: {
    statPercent(stat) {
      return stat / 255 * 100;
    }
  }
};
</script>
