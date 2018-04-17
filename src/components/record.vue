<template>
  <v-app light id="inspire">
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

		<v-content>
      <v-container fluid>
        <v-slide-y-transition mode="out-in">
          <v-layout column align-left>
            <v-breadcrumbs>
              <v-icon slot="divider">chevron_right</v-icon>
              <v-breadcrumbs-item
                v-for="item in breadcrumbs" :key="item.text"
                :disabled="item.disabled"
              >
                {{ item.text }}
              </v-breadcrumbs-item>
            </v-breadcrumbs>

            <h1>Blood Request</h1>
            <br>
            <v-card style='padding: 1em'>
              <v-layout justify-center column>
                <v-card>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs1>
                        <v-avatar
                          size="50px"
                          slot="activator"
                        >
                          <img
                            src="https://www.what-dog.net/Images/faces2/scroll007.jpg"
                            alt=""
                            v-if="current_pet.img"
                          >
                          <v-icon v-else>{{ current_pet.img }}</v-icon>
                        </v-avatar>
                      </v-flex>
                      <v-flex xs3>
                        <v-chip
                          label
                          small
                          class="ml-0"
                          v-if="current_pet.petName"
                        >{{current_pet_detail.patient_species}}</v-chip>
                        <br>
                        <h2 v-html="current_pet.petName"/>
                        <span class="grey--text" v-if="current_pet.petID">&nbsp;(ID: {{current_pet.petID}})</span>
                      </v-flex>
                      <v-flex xs4>
                        <b>เพศ:</b> {{current_pet_detail.patient_gender}}<br>
                        <b>วันเกิด:</b> {{current_pet_detail.patient_birth_dt}}<br>
                      </v-flex>
                      <v-flex xs4>
                        <b>เจ้าของ:</b> {{current_pet_owner.owner_name}}<br>
                        <b>เบอร์ติดต่อ:</b> {{current_pet_owner.TelNumer}}<br>
                      </v-flex>
                    </v-layout>

                  </v-card-text>
                </v-card>
              </v-layout>
              <br>
              <div class="text-xs-center">
                <a href='/request'>
                  <v-btn round dark>กลับไปหน้าขอเลือด</v-btn>
                </a>
              </div>
            </v-card>
          </v-layout>
        </v-slide-y-transition>
      </v-container>
    </v-content>

    <app-footer></app-footer>

  </v-app>
</template>

<script>
	/* eslint-disable */

  import AppFooter from '@/components/common/Footer.vue'
  import axios from 'axios'

  export default {
    components: {
      AppFooter
    },
    data () {
      return {
        postBody: '',
        errors: [],
        current_pet: [],
        current_pet_detail: [],
        current_pet_owner: [],
        clipped: false,
        drawer: false,
        clipped: false,
				breadcrumbs: [
          {
            text: 'การเก็บเลือด',
            disabled: false
          },
          {
            text: 'การบริจาคเลือด',
            disabled: true
          }
        ],
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
				date: null,
	      dateFormatted: null,
	      menu: false,
				date2: null,
	      dateFormatted2: null,
	      menu2: false,
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Smart Pet Blood Bank',
      }
    },
		methods: {
      formatDate (date) {
        if (!date) {
          return null
        }

        const [year, month, day] = date.split('-')
        return `${month}/${day}/${year}`
      },
      parseDate (date) {
        if (!date) {
          return null
        }

        const [month, day, year] = date.split('/')
        return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
      },
      getDetail () {
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/results', {
          petID : this.selected_request.pet_id
        },headers)
        .then(response => {
					this.current_pet = response.data
          this.current_pet.img = 'https://www.what-dog.net/Images/faces2/scroll007.jpg'
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/pets/detail', {
          petID : this.selected_request.pet_id
        },headers)
        .then(response => {
					this.current_pet_detail = response.data
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/owners/detail', {
          ownerID : '1'
        },headers)
        .then(response => {
					this.current_pet_owner = response.data
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/request/specificrequestwithrequestdetail', {
          "RequestID": this.selected_request.request_id
        },headers)
        .then(response => {
          this.selected_bags = response.data
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    },
    }
  }
</script>


<style lang="scss">
  .color__mdred {
    color: #df5249 !important;
  }

  .bg__mdteal {
    background-color: #5c9ba4 !important;
  }

  .navbar {
    background-color: #df5249 !important;
    color: white;
  }
  a{
    text-decoration: none;
  }

  .img_task{
    max-height: 150px;
    max-width: 150px;
    margin: 2em 2em 0em 2em;
  }
  .caption_task{
    font-size: 1.4em;
    font-weight: bold;
  }

</style>
