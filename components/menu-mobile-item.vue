<template>
  <section>
    <v-list-group
      :prepend-icon="menu.icon"
    >
      <template #activator>
        <v-list-item-title>
          {{ menu.title }}
        </v-list-item-title>
      </template>
      <v-list-item
        v-for="(item, index) in menu.items"
        :key="index"
        :to="item.to"
        color="primary"
        router
      >
        <v-list-item-title>{{ item.title }}</v-list-item-title>
      </v-list-item>
    </v-list-group>
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
