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
      >
        <template v-slot:body="{ items, headers }">
          <tbody>
            <tr v-for="(item,idx,k) in items" :key="idx">
              <td v-for="(header,key) in headers" :key="key">
                <v-edit-dialog
                > {{item[header.value]}}
                  <template v-slot:input>
                    <v-text-field
                      @change="update(item._id, header.value, $event)"
                      :value="item[header.value]"
                      label="Edit"
                      single-line
                    ></v-text-field>
                  </template>
                </v-edit-dialog>
              </td>
            </tr>
          </tbody>
        </template>
        <!-- <template v-for="header in headers" v-slot:header.value="props">
          <v-edit-dialog
            :return-value.sync="props.value"
            @save="save"
            @cancel="cancel"
            @open="open"
            @close="close"
          >
            {{ props.value}}
            <template v-slot:input>
              <v-text-field
                v-model="item"
                :rules="[max25chars]"
                label="Edit"
                single-line
                counter
              ></v-text-field>
            </template>
          </v-edit-dialog>
        </template> -->
      </v-data-table>
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
    update(_id, field, value) {
      let data = {}
      data[field] = value

      let params = {}

      this.$store.dispatch([this.service]+ '/patch', [_id, data, params])
    }
  }
}
</script>
