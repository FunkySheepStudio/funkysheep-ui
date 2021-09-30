<template>
  <section>
    <v-card>
      <v-card-title>
        Parameters
      </v-card-title>
      <v-card-text>
        Endpoint: {{ service }}
      </v-card-text>
    </v-card>
    <v-card>
      <v-card-title>
        Data
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="list.data"
      />
    </v-card>
    Fields
    <v-data-table
      :headers="fieldsHeaders"
      :items="fieldsData"
    />
  </section>
</template>
<script>
import { mapActions, mapGetters } from 'vuex'
export default {
  props: {
    service: {
      type: String,
      default: ''
    }
  },
  computed: {
    //  ...mapGetters('/api/system/services', { find: 'find', get: 'get' }),
    list() {
      return this.$store.getters[this.service + '/find']();
    },
    fields () {
      return this.list.data.flatMap((value) => {return Object.keys(value)})
        .reduce((unique, item) => (unique.includes(item) ? unique : [...unique, item]),[],)
    },
    fieldsHeaders () {
      return [{
        value: '_id',
        text: 'ID'
      },
      {
        value: 'name',
        text: 'Name'
      },
      {
        value: 'save',
        text: 'Save'
      }
      ]
    },
    fieldsData () {
      return this.fields.map((value) => {
          return {
            _id: value,
            name: '',
            save: false
          }
        })
    },
    headers () {
      return this.fields.map((value) => {
        return {
          text: value,
          value
        }
      })
    }
  },
  mounted() {
    this.$store.dispatch([this.service]+ '/find')
  },
  methods: {
    //  ...mapActions('/api/system/services', { findService: 'find' })
  }
}
</script>
