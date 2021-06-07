<template>
  <section>
    <v-card
      max-width="150"
    >
      <v-card-title>
        {{ name }}
      </v-card-title>
      <v-card-actions class="justify-center">
        <v-progress-circular
          width="15"
          :color="color"
          :size="100"
          :value="value"
        >
          {{ total }}
        </v-progress-circular>
      </v-card-actions>
    </v-card>
  </section>
</template>
<script>
import { mapActions, mapGetters } from 'vuex'
export default {
  components: {},
  props: {
    name: {
      type: String,
      default: ''
      },
    color: {
      type: String,
      default: 'red'
      },
    total: {
      type: Number,
      default: 100
      },
    value: {
      type: Number,
      default: 0
    }
  }
}
</script>
<style>
</style>
