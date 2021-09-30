<template>
  <section>
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="red"
          elevation="2"
          fab
          absolute
          bottom
          right
          v-on="on"
          v-bind="attrs"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Service: {{ service }}
        </v-card-title>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </section>
</template>

<script>
export default {
  props: {
    service: {
      type: String,
      default: ''
    },
    _id: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      dialog: false,
    }
  },
  methods: {
    save() {
      if (_id === '') { // Create a record

      } else { // Update the record

      }
    }
  }
}
</script>
