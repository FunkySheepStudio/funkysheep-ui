<template>
  <section>
    <v-btn
      fab
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
      v-model="showForm"
      :persistent="!$store.state.auth.user"
    >
      <v-tabs
        v-if="!$store.state.auth.user"
      >
        <v-tab>
          Login
        </v-tab>
        <v-tab-item>
          <v-card>
            <v-card-text>
              {{ $data._id }}
              <v-text-field
                v-model="login"
                label="Login"
              />
              <v-text-field
                v-model="password"
                label="Password"
                :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                :type="showPassword ? 'text' : 'password'"
                @click:append="showPassword = !showPassword"
              />
              <v-btn
                @click="Login()"
              >
                Login
              </v-btn>
              {{ message }}
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab>
          Create Account
        </v-tab>
        <v-tab-item>
          <v-card>
            <v-card-text>
              <funkysheep-user-profile/>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs>
      <v-card
        v-if="$store.state.auth.user"
      >
        <v-card-title>
          Profile
        </v-card-title>
        <v-card-text>
          <funkysheep-user-profile/>
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
      message: '',
      isAuthenticated: false
    }
  },
  computed: {
    ...mapGetters('/api/system/users', { users: 'find', get: 'get' })
  },
  mounted () {
    //  Create the local storage if the user do not exist localy
    /*if (!localStorage.getItem('_id')) {
      localStorage.setItem('_id', Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15))
      localStorage.setItem('login', Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15))
      localStorage.setItem('password', Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15))
    }*/

    this._id = localStorage.getItem('_id')
    this.login = localStorage.getItem('login')
    this.password = localStorage.getItem('password')

    // Create the database user if the user do not exist in database
    /*this._idExist(localStorage.getItem('_id'))
      .then((exist) => {
        if (!exist) {
          this.create({
            _id: localStorage.getItem('_id'),
            login: this.login,
            password: this.password
          })
            .then(() => {
              this.auth()
            })
        } else {
          this.auth()
        }
      })*/
      this.Auth()
  },
  methods: {
    ...mapActions('/api/system/users', { findUsers: 'find', create: 'create', patch: 'patch' }),
    Auth () {
      if (!this.$store.getters['auth/isAuthenticated']) {
        if (localStorage.getItem('feathers-jwt')) { //  Try the jwt auth
          this.$store.dispatch('auth/authenticate', {
            accessToken: localStorage.getItem('feathers-jwt'),
            strategy: 'jwt'
          })
          .then((auth) => {
            this.showForm = false
          })
          .catch(() => {
            console.log('Cannot Auth jwt')
            this.message = err
          })
        } else {
          if (this.login && this.password)
          {
            this.$store.dispatch('auth/authenticate', { //  Try the local auth
            login: this.login,
            password: this.password,
            strategy: 'local'
            })
            .then((auth) => {
              localStorage.setItem('_id', auth.user._id)
              this._id = auth.user._id
              this.showForm = false
            })
            .catch((err) => {
              this.showForm = true;
              console.log('Cannot Auth local')
              this.message = err
            })
          } else {
            this.showForm = true;
          }
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
    Login () {
      localStorage.setItem('login', this.login)
      localStorage.setItem('password', this.password)
      localStorage.removeItem('feathers-jwt')
      this.Auth()
      
    }
  }
}
</script>
