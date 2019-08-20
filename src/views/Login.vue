<template>


      <v-card class="app-body" flat>
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
            <p>{{serverErrors}}</p>
          </v-alert>

        </div>
          <v-layout row wrap>
            <v-flex xs12>
              <v-card color="secondary" class="white--text">
                  <img class="my_logo" src="../assets/car-rental.svg"/>
                <v-card-title primary-title>
                  <div class="headline">Welcome to MotPort </div>
                  <!--<div> You can login or signup for an account to begin using the courier service</div> -->
                </v-card-title>
                <v-card-text>
                  <v-form ref="form" v-model="valid" lazy-validation>
                    <v-text-field
                      v-model="username"
                      :rules="usernameRules"
                      label="EMAIL"
                      required
                      placeholder="Email address"
                      single-line
                    ></v-text-field>
                    <v-text-field
                      v-model="password"
                      :rules="passwordRules"
                      label="PASSWORD"
                      type="password"
                      required
                      placeholder="Password"
                      single-line
                    ></v-text-field>
                    <div class="login_links"><a href="#">Forgot your password?</a></div>


                    <v-btn
                      :disabled="!valid"
                      @click="submit"
                      color="rgba(245,83,0,0.75)"
                      block
                      light
                      round
                      depressed
                    >
                      LOG IN
                    </v-btn>

                    <div class="signup_links">Don't have an account? <a href="#"> SIGN UP</a></div>

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
/* eslint-disable */
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
        this.$axios.post('/login', { username: this.username, password: this.password })
          .then((response) => {
            if (response.data.status !== 'error') {
              let accessToken = response.data.auth.access_token
              localStorage.setItem('token', accessToken)
              localStorage.setItem('user', response.data.user)
              this.alertMessage = response
              this.alert = true
              this.$router.push({ path: '/' })

            }
            else if (response.data.status == 'error')
            {
              if(response.data.message == 'User not Verified') {
                this.$router.push({ name: 'Activate', params: { codeSent: '1' } })
              }
              else {
                console.log(response.data.message)
                this.errors.record(response.data.message)
                this.serverErrors = response.data.message
                this.alert = true
              }
            }
          })/*
          .catch((error) => {
            console.log(error)
            this.errors.record(error)
            this.serverErrors = error.data.errors
            this.alert = true
          })*/
      }
    },
    clear () {
      this.$refs.form.reset()
    }
  },
  mounted () {
    // do something after mounting vue instance
    if (this.$token) {
      this.$router.push({ path: '/' })
    }
  }
}
</script>



/* Page CSS Code  */

<style scoped>

.app_menu {display: none;}

.theme--dark.accent {
  background: #ffffff !important;
}

.v-content, .v-content__wrap {
  background: #ffffff !important;
}

.secondary {
    background-color: transparent !important;
    box-shadow: none;
}

/* #logo img {width: 50%;} */

.white--text {
  text-align: center;
}

.headline {
  text-align: center;
  margin: 0 auto;
  font-weight: 300;
  color: #919191;
  font-size: 16px !important;
}

.v-input .v-label {
  font-size: 14px !important;
}

.v-input__slot {
    border: 1px solid rgba(0,0,0,.3) !important;
}

.theme--light.v-btn {color: #ffffff !important;}

.login_links {margin-bottom:15px;}

.login_links a {
  font-size: 11px;
  letter-spacing: 0.5px;
  text-decoration: none;
  color: #f55300;
}

.signup_links {
  color: #919191;
  text-align: center;
  margin-top: 20px;
  letter-spacing: 0.5px;
}

.signup_links a {
  text-decoration: none;
  color: #f55300;
  letter-spacing: 0.5px;
}


</style>
