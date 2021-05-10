<template>
  <v-img
    class="Logo"
    alt="Funky Sheep Logo"
    src="/img/Logo-Mini.png"
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
