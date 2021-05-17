<template>
  <section>
    <v-divider
      class="mx-1"
      vertical
    />
    <v-menu
      offset-y
      transition="slide-y-transition"
      rounded="lg"
    >
      <template #activator="{ on, attrs }">
        <v-btn
          color="secondary"
          rounded
          v-bind="attrs"
          v-on="on"
        >
          <v-icon
            dark
            left
          >
            {{ menu.icon }}
          </v-icon>
          {{ menu.title }}
        </v-btn>
      </template>
      <v-list>
        <v-list-item
          v-for="(item, index) in menu.items"
          :key="index"
          :to="item.to"
          color="primary"
          router
        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </section>
</template>

<script>
export default {
  props: {
    menu: {
      type: Object,
      default: () => {
        return {
          title: 'None',
          icon: 'mdi-cloud-upload',
          items: []
        }
      }
    }
  }
}
</script>
