<template>
  <div>
    <h1>
      Missing Pokemon Id on Route
    </h1>
    <v-btn
      class="mr-4 mt-6"
      :to="{name: 'index'}"
    >
      Back to Home
    </v-btn>
  </div>
</template>

<script>
export default {
  name: "pokemon"
}
</script>

<style scoped>
h1 {
  color: red;
}
</style>
