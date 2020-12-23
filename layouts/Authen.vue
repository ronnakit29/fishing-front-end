
<template>
  <v-app>
    <v-container>
      <nuxt />
    </v-container>
    <v-footer
      color="white"
      padless
      absolute
    >
      <v-row
        justify="center"
        no-gutters
      >
        <v-col
          class="lighten-2 py-4 text-center"
          cols="12"
        >
          {{ new Date().getFullYear() }} — <strong>FOR FISH FARM TEAM</strong>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'Authen',
  created () {

  },
  data () {
    return {
    }
  },
  props: {

  },
  methods: {

  },
}
</script>

<style lang="scss" scoped>
</style>

