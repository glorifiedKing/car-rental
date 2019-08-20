<template>
      <v-card>
        <v-container
          fluid
          grid-list-lg
        >
        <div>
    <v-alert
      v-model="alert"
      dismissible
      type="error"
    >
      <ul>
        <li v-for ="(index,flash) in serverErrors" :key="index" v-text="flash"></li>
      </ul>
    </v-alert>

  </div>
          <v-layout row wrap>
            <v-flex xs12>
              <v-card color="secondary">
                <v-card-title primary-title>
                  <div class="headline">Sign Up </div>
                  <div class="sub-headline"> Please fill in your details to create an account and begin using the courier service</div>
                </v-card-title>
                <v-card-text>
                  <v-form ref="form" v-model="valid" lazy-validation>
                    <v-text-field
                      v-model="name"
                      :rules="nameRules"
                      :counter="100"
                      :error-messages="errors.get('name')"
                      label="Full name"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="username"
                      :rules="usernameRules"
                      :counter="21"
                      :messages="errors.get('username')"
                      label="Username"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="password"
                      :rules="[v => !!v || 'Password is required']"
                      label="Password"
                      type="password"
                      required
                    ></v-text-field>
                    <v-layout row wrap>
                    <v-flex xs4 sm4 md3>
                      <v-select
                        v-model="select"
                        :items="countries"
                        item-text="name"
                        item-value="code"
                        :rules="[v => !!v || 'Country is required']"
                        label="Country"
                        required
                      ></v-select>
                    </v-flex>
                    <v-flex xs6 sm6 md7>
                    <v-text-field
                      v-model="phone_number"
                      :rules="numberRules"
                      :counter="10"
                      :messages="errors.get('phone_number')"
                      label="Phone number"
                      placeholder="0772568745"
                      required
                    ></v-text-field>
                  </v-flex>
                </v-layout>
                    <v-text-field
                      v-model="email"
                      :rules="emailRules"
                      :error-messages="errors.get('email')"
                      label="E-mail"
                      required
                    ></v-text-field>

                    <v-checkbox
                      v-model="checkbox"
                      :rules="[v => !!v || 'You must agree to continue!']"
                      label="I agree to the SGA Courier terms and conditions?"
                      required
                    ></v-checkbox>

                    <v-btn
                      :disabled="!valid"
                      @click="submit"
                      color="rgba(245,83,0,0.75)"
                      light
                      round

                    >
                      submit
                    </v-btn>
                    <v-btn @click="clear"
                    light
                    round
                    ><span class="primary--text">clear</span></v-btn>
                  </v-form>
             </v-card-text>
                <v-card-actions>

                </v-card-actions>
              </v-card>
            </v-flex>



          </v-layout>
        </v-container>
      </v-card>

</template>
<script>
/* eslint-disable no-console */
class Errors {
  constructor () {
    this.errors = {}
  }
  get (field) {
    if (this.errors[field]) {
      return this.errors[field][0]
    }
  }
  record (errors) {
    this.errors = errors
  }
}
export default {
  data () {
    return {
      valid: false,
      alert: false,
      serverErrors: {},
      errors: new Errors(),
      alertMessage: '',
      usernameRules: [
        (v) => !!v || 'Username is required',
        (v) => v.length > 4 && v.length <= 21 || 'User Name must be greater than 4 characters and less than 20'
      ],
      nameRules: [
        (v) => !!v || 'Name is required',
        (v) => v && v.length >= 10 || 'Name must be more than 10 characters'
      ],
      numberRules: [
        (v) => !!v || 'Number is required',
        (v) => v && !isNaN(v) || 'Not a valid phone number',
        (v) => v.length > 8 && v.length <= 10 || 'Number must be 9 or 10 digits'
      ],
      emailRules: [
        (v) => !!v || 'E-mail is required',
        (v) => /^\w+([-]?\w+)*@\w+([-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
      ],
      countries: [
        {id: 1, name: 'Uganda', code: '256'},
        {id: 2, name: 'Kenya', code: '254'},
        {id: 3, name: 'Tanzania', code: '255'}
      ]
    }
  },
  methods: {
    submit () {
      this.$refs.form.validate()
      if (this.valid === true) {
        this.$axios.post('/register', { username: this.username, email: this.email, name: this.name, phone_number: this.phone_number, password: this.password, country: this.select })
          .then((response) => {
            console.log(response)
            let accessToken = response.data.auth.access_token
            localStorage.setItem('token', accessToken)
            localStorage.setItem('user', response.data.user)
            this.alertMessage = response
            this.alert = true
            this.$token = accessToken
            this.$router.push({ name: 'Activate', params: { codeSent: '1' } })
          })
          .catch((error) => {
            console.log(error)
            this.errors.record(error.response.data)
            this.serverErrors = error.response.data.errors
            this.alert = true
          })
      }
    },
    clear () {
      this.$refs.form.reset()
    }
  }
}
</script>

/* Page CSS Code  */

<style scoped>

.secondary {
    background-color: transparent !important;
    box-shadow: none;
}

.headline {
  font-weight: 300;
  color: #F55300;
  font-size: 20px !important;
  margin-bottom: 1em;
}

.sub-headline {
  font-weight: 300;
  color: #919191;
}

.theme--light.v-btn {
  color: #ffffff !important;
  letter-spacing: 1px;
}





</style>
