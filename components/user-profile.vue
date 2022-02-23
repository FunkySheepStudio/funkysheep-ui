<template>
  <section>
    <v-card>
      <v-card-text>
        {{ $data._id }}
        <v-text-field
          v-model="login"
          label="Login"
          @input="CheckLogin($event)"
        />
        <v-text-field
          v-model="password"
          label="Password"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          :type="showPassword ? 'text' : 'password'"
          @click:append="showPassword = !showPassword"
        />
        <v-text-field
          v-model="passwordCheck"
          label="Confirm Password"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          :type="showPassword ? 'text' : 'password'"
          @click:append="showPassword = !showPassword"
        />
        <v-btn
          v-if="$store.state.auth.user"
          @click="Logout()"
        >
          Logout
        </v-btn>
        <v-btn
          v-if="isValid"
          @click="Save()"
        >
          Save
        </v-btn>
        {{ message }}
      </v-card-text>
    </v-card>
  </section>
</template>
<script>
import { mapActions, mapGetters } from 'vuex'
export default {
  data () {
    return {
      _id: '',
      password: '',
      passwordCheck: '',
      login: '',
      showPassword: false,
      message: '',
      isValid: false
    }
  },
  computed: {
    ...mapGetters('/api/system/users', { users: 'find', get: 'get' })
  },
  mounted () {
    if (this.$store.state.auth.user)
    {
      this._id = this.$store.state.auth.user._id
      this.login = this.$store.state.auth.user.login
      this.isValid = true;
    } else {
      this._id = ''
      this.login = ''
      this.isValid = false;
    }
  },
  methods: {
    ...mapActions('/api/system/users', { findUsers: 'find', patch: 'patch', create: 'create' }),
    LoginExist (login) {
      var query = {
          login,
          _id: {
            $ne: this._id
          }
        }

      return this.findUsers({
        query
      })
        .then((users) => {
          console.log(users);
          if (users.total === 1) {
            return true
          } else {
            return false
          }
        })
    },
    CheckLogin (login)
    {
      this.LoginExist(login)
      .then((result) => {
        if (result)
        {
          this.isValid = false
          this.message = "Login already in use"
        } else {
          this.isValid = true
          this.message = ""
        }
      })
    },
    Save () {

      if (this._id)
      {
        this.patch([
        this._id,
        {
          login: this.login,
          password: this.password
        }
        ])
        .then(() => {
          localStorage.setItem('login', this.login)
          localStorage.setItem('password', this.password)
        })
        .catch((err) => {
          this.message = err.message
        })
      } else {
        var data = {
          login: this.login,
          password: this.password
        }
        this.create(data)
      }
    },
    Logout()
    {
      this.$store.dispatch('auth/logout')
        .then(() => {
          localStorage.setItem('login', this.login)
          localStorage.setItem('password', this.password)
          localStorage.removeItem('feathers-jwt')
      })
    }
  }
}
</script>
