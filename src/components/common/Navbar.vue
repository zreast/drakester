<template>
  <v-app>
  <v-navigation-drawer
    fixed
    :clipped="$vuetify.breakpoint.width > 1264"
    v-model="drawer"
    app
  >
    <v-list>
      <v-list>
        <v-list-tile v-for="item in items2" :key="item.text" avatar @click="">
          <v-list-tile-avatar>
            <img :src="`https://randomuser.me/api/portraits/men/${item.picture}.jpg`" alt="">
          </v-list-tile-avatar>
          <v-list-tile-title v-text="item.text"></v-list-tile-title>
        </v-list-tile>
      </v-list>
      <v-list-tile v-for="item in items" :key="item.text" @click="">
        <v-list-tile-action class="grey--text darken-4">
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-tile-action>
        <v-list-tile-content class="black--text">
          <v-list-tile-title v-text="item.title"></v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>
      <v-list-tile @click="">
        <v-list-tile-action>
          <v-icon color="grey darken-1">settings</v-icon>
        </v-list-tile-action>
        <v-list-tile-title class="grey--text text--darken-1">Settings</v-list-tile-title>
      </v-list-tile>
    </v-list>
  </v-navigation-drawer>

  <v-toolbar app class="navbar white--text">
    <v-toolbar-side-icon @click.stop="drawer = !drawer" class="white--text"></v-toolbar-side-icon>
    <v-toolbar-title v-text="title"></v-toolbar-title>
    <v-spacer></v-spacer>
  </v-toolbar>
</v-app>
</template>

<script>
  /* eslint-disable */
  export default {
    data () {
      return {
        clipped: false,
        drawer: false,
        fixed: false,
        items: [
          { title: 'Dashboard', icon: 'dashboard' },
          { title: 'Do it now!', icon: 'extension' },
          { title: 'Statistic', icon: 'trending_up' },
          { title: 'Email', icon: 'email' },
          { title: 'Social', icon: 'message' }
        ],
        items2: [
          { picture: 28, text: 'โรงพยาบาลสัตว์ สวนผัก' }
        ],
        right: true,
        rightDrawer: false,
        title: 'iTaam CRM tools for Animal Hospital',
      }
    }
  }
</script>

<style lang="scss">

</style>
