<template>
  <section>
    <v-btn
      :hidden="!$vuetify.breakpoint.mobile"
      fab
      elevation="0"
      x-small
      color="primary"
      @click="mobileMenu = !mobileMenu"
    >
      <v-icon dark>
        mdi-menu
      </v-icon>
    </v-btn>
    <v-btn
      :hidden="!$vuetify.breakpoint.mobile"
      fab
      elevation="0"
      x-small
      color="primary"
      @click="mobileMenu = !mobileMenu"
    >
      <v-icon dark>
        mdi-discord
      </v-icon>
    </v-btn>
  </section>
</template>
<script>
export default {
  props:{
    src: {
      type: String,
      default: '/img/logo.png'
    },
    to: {
      type: String,
      default: '/'
    }
  }
}
</script>
