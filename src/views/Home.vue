<template>
  <v-container fluid fill-height ma-0 pa-0>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>

      </v-layout>
    </v-slide-y-transition>

</v-container>
</template>

<script>

export default {
  data () {
    return {
      valid: false,
      alert: false,
      textRules: [
        (v) => !!v || 'Value is required',
        (v) => v && v.length >= 5 || 'Value must be more than 5 characters'
      ],
    }
  },
  mounted() {
    if(!this.$token) {
        this.$router.push({ name: 'Login', params: { codeSent: '1' } })
    }
    else if(this.$token)
    {

    }

  },
  methods: {
    submit () {
      this.$refs.form.validate()
      if (this.valid === true) {
          this.$router.push({ name: 'BookingDetail', params: { sender_address: this.pickupLocationName, destination_address: this.destinationLocationName, distance: this.distanceBetweenLocations, pickup_coordinates: this.pickupLocation.latitude+","+this.pickupLocation.longitude,destination_coordinates: this.destinationLocation.latitude+","+this.destinationLocation.longitude } })

      }
    },
    clear () {
      this.$refs.form.reset()
    }

}

}
</script>
