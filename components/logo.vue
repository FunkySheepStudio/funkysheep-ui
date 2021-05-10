<template>
  <v-img
    class="Logo"
    alt="Funky Sheep Logo"
    :src="src"
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
