<template>
  <v-img
    class="Logo"
    alt="Funky Sheep Logo"
    :src="src"
    max-height="50"
    max-width="50"
  />
</template>
<script>
export default {
  props: {
    src: {
      type: String,
      default: '/img/logo.png'
      }
    }
  }
</script>
