/* eslint-disable */
<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item
          v-for="item in items"
          :key="item.text"
          @click=""
        >
          <v-list-item-action>

          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ item.text }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-subheader class="mt-4 grey--text text--darken-1">SUB</v-subheader>
        <v-list>
          <v-list-item
            v-for="item in items2"
            :key="item.text"
            @click=""
          >
            <v-list-item-avatar>
              <img
                :src="`https://randomuser.me/api/portraits/men/${item.picture}.jpg`"
                alt=""
              >
            </v-list-item-avatar>
            <v-list-item-title v-text="item.text"></v-list-item-title>
          </v-list-item>
        </v-list>

      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
      color="red"
      dense
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-icon class="mx-4">fab fa-youtube</v-icon>
      <v-toolbar-title class="mr-12 align-center">
        <span class="title">Chariot</span>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>

    </v-app-bar>

    <v-content>
      <v-container >
        <v-row>
          <v-card
           color="#385F73"
           dark
         >
           <v-card-text class="white--text">
             <div class="headline mb-2">Pick from</div>
             ....
           </v-card-text>

           <v-card-actions>
             <v-btn text>...</v-btn>
           </v-card-actions>
         </v-card>

        </v-row>
        <v-row>
          <v-card
           color="#385F73"
           dark
         >
           <v-card-text class="white--text">
             <div class="headline mb-2">Return to</div>
             ....
           </v-card-text>

           <v-card-actions>
             <v-btn text>...</v-btn>
           </v-card-actions>
         </v-card>

        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  export default {
    props: {
      source: String,
    },
    data: () => ({
      drawer: null,
      items: [
        { icon: 'trending_up', text: 'Most Popular' },
        { icon: 'subscriptions', text: 'Subscriptions' },
      ],
      items2: [

      ],
    }),
    created () {
      this.$vuetify.theme.dark = true
    },
  }
</script>
