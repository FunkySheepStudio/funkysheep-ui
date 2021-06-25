<template>
  <section>
    <v-btn
      fab
      color="white"
      small
      target="_blank"
      @click="showForm = !showForm"
    >
      <v-icon
        large
      >
        mdi-account
      </v-icon>
    </v-btn>
    <v-dialog
      :value="showForm"
      persistent
    >
      <v-card>
        <v-card-title>
          User
        </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="login"
            label="Login"
            @input="setMode($event)"
          />
          <v-text-field
            v-model="password"
            label="Password"
            :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
            :type="showPassword ? 'text' : 'password'"
            @click:append="showPassword = !showPassword"
          />
          <v-btn
            @click="process()"
          >
            {{ mode }}
          </v-btn>
        </v-card-text>
      </v-card>
    </v-dialog>
  </section>
</template>
<script>
import { mapActions, mapGetters } from 'vuex'
export default {
  data () {
    return {
      showForm: false,
      _id: '',
      password: '',
      login: '',
      showPassword: false,
      mode: 'Save'
    }
  },
  computed: {
    ...mapGetters('users', { users: 'find', get: 'get' })
  },
  mounted () {
    //  Create the local storage if the user do not exist localy
    if (!localStorage.getItem('_id')) {
      localStorage.setItem('_id', Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15))
      localStorage.setItem('login', '')
      localStorage.setItem('password', Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15))
    }

    this._id = localStorage.getItem('_id')
    this.login = localStorage.getItem('login')
    this.password = localStorage.getItem('password')

    // Create the database user if the user do not exist in database
    this._idExist(localStorage.getItem('_id'))
      .then((exist) => {
        if (!exist) {
          this.create({
            _id: localStorage.getItem('_id'),
            login: '',
            password: localStorage.getItem('password')
          })
          .then((user) => {
            this.login = user.login
            this.auth()
          })
        } else {
          this.auth()
        }
      })
  },
  methods: {
    ...mapActions('users', { findUsers: 'find', create: 'create', patch: 'patch' }),
    auth () {
      if (!this.$store.getters['auth/isAuthenticated']) {
        if (localStorage.getItem('feathers-jwt')) { //  Try the jwt auth
          this.$store.dispatch('auth/authenticate', {
            accessToken: localStorage.getItem('feathers-jwt'),
            strategy: 'jwt'
          })
          .catch(() => {
            console.log('Cannot Auth jwt')
          })
        } else if (this.password !== null) {
          if (this.login = '') {

          }
          
          this.$store.dispatch('auth/authenticate', { //  Try the local auth
            _id: localStorage.getItem('login'),
            password: localStorage.getItem('password'),
            strategy: 'local'
          })
          .catch(() => {
            console.log('Cannot Auth local')
          })
        } else { // Password not saved, show the login primpt
          this.showForm = true
        }
      }
    },
    _idExist (_id) {
      return this.findUsers({
        query: {
          _id
        }
      })
        .then((users) => {
          if (users.total === 1) {
            return true
          } else {
            return false
          }
        })
    },
    loginExist (login) {
      return this.findUsers({
        query: {
          login
        }
      })
        .then((users) => {
          if (users.total === 1) {
            return true
          } else {
            return false
          }
        })
    },
    setMode (login) {
      this.loginExist(login)
        .then((exist) => {
          if (exist) {
            this.mode = 'Login'
          } else {
            this.mode = 'Save'
          }
        })
    },
    process () {
      this.$store.dispatch('auth/logout')

      if (this.mode = 'Save') {
        this.patch([
          localStorage.getItem('login'),
          {
            _id: this.login,
            password: this.password,
            guest: false
          }
        ])
      }

      this.auth()

      localStorage.setItem('login', this.login),
      localStorage.setItem('password', this.password),
      localStorage.removeItem('feathers-jwt')

      this.showForm = false
    }
  }
}
</script>
