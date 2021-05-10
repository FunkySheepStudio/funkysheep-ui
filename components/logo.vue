<template>
  <router-link
    :to="to"
  >
    <v-img
      class="Logo"
      alt="Funky Sheep Logo"
      :src="src"
      max-height="50"
      max-width="50"
    />
  </router-link>
</template>
<script>
export default {
  props: {
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
