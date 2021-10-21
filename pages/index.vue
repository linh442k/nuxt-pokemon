<template>
  <div>
    <h1>Choose Your Pokemon</h1>
    <validation-observer ref="observer" v-slot="{ invalid }">
        <!--        v-on:fetch-data="fetchData"  -->
        <select-box
          v-model="pokemonUrl"
          :label="'Select Pokemon'"
          :rules="'required'"
          :limit="10"
          :offset="10"
          :default-api="'https://pokeapi.co/api/v2/pokemon?'"
          :item-text="'name'"
          :item-value="'url'"
        />
      <v-btn
        class="mr-4"
        :disabled="invalid"
        :to="{name: 'pokemon-id', params: {id: pokemonID}}"
      >
        Check Pokemon Stats
      </v-btn>
    </validation-observer>
  </div>
</template>
<script>
import SelectBox from "../components/select-box";

export default {
  components: { SelectBox },
  data: () => ({
    pokemonUrl: "",
    pokemonList: [],
  }),
  computed:
    {
      pokemonID: function() {
        return this.pokemonUrl.length === 0 ? "" : this.pokemonUrl.split("/")[this.pokemonUrl.split("/").length - 2];
      },
    },
};
</script>
