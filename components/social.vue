<template>
  <section>
    <v-btn
      fab
      elevation="0"
      x-small
      color="primary"
      href="https://github.com/FunkySheepStudio/"
      target="_blank"
      :hidden="$vuetify.breakpoint.mobile"
    >
      <v-icon dark>
        mdi-github
      </v-icon>
    </v-btn>
    <v-btn
      fab
      elevation="0"
      x-small
      color="#BA68C8"
      href="https://discord.gg/9PWWDHPeJ9"
      target="_blank"
      :hidden="$vuetify.breakpoint.mobile"
    >
      <v-icon dark>
        mdi-discord
      </v-icon>
    </v-btn>
  </section>
</template>
<script>
export default {
}
</script>
