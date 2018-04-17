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
        <v-list-tile v-for="item in items" :key="item.text" @click="route(item.page)">
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

            <h1 style='color:#1976D2'><v-icon color="blue darken-2">local_hospital
            </v-icon> Blood Request Board</h1>
            <br>
            <v-container grid-list-md text-xs-center>
              <v-layout row wrap>
                <v-flex xs3>
                  <v-card>
                    <v-card-text class="px-0 caption_task">New Request</v-card-text>
                    <span class='stat_task'>{{requests.requests.length}}</span>
                  </v-card>
                </v-flex>
                <v-flex xs3>
                  <v-card>
                    <v-card-text class="px-0 caption_task">Crossmatch Stage</v-card-text>
                    <span class='stat_task'>N/A</span>
                  </v-card>
                </v-flex>
                <v-flex xs3>
                  <v-card>
                    <v-card-text class="px-0 caption_task">จ่ายถุงเลือดแล้ววันนี้</v-card-text>
                    <span class='stat_task'>{{requests.requests.length}}</span>
                  </v-card>
                </v-flex>
                <v-flex xs3>
                  <v-card>
                    <v-card-text class="px-0 caption_task">Today in Process</v-card-text>
                    <span class='stat_task'>{{requests.requests.length}}</span>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>

          </v-layout>
        </v-slide-y-transition>

        <br>
        <v-layout row wrap>
          <v-spacer></v-spacer>
          <v-text-field
            append-icon="search"
            label="Search by name"
          ></v-text-field>
          <v-btn
            color="grey"
            class="white--text"
            @click="route('newrequest')"
          >
            <v-icon dark>add</v-icon>
            New Request
          </v-btn>
        </v-layout>
        <br>
        <br>
        <v-card v-for='item in requests.requests' style='margin-bottom: 1em'>
          <v-card-actions>
            <v-container grid-list-md text-xs-center>
              <v-layout row wrap>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      สถานะ
                    </v-card-text>
                    <v-card-text class="px-0">
                      <v-chip
                        label
                        small
                        class="white--text"
                        color="yellow darken-2"
                      >Request Submit</v-chip>
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      Request Date
                    </v-card-text>
                    <v-card-text class="px-0">
                      {{item.date}}
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      Recipient
                    </v-card-text>
                    <v-card-text class="px-0">
                      {{item.petName}}
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      Owner
                    </v-card-text>
                    <v-card-text class="px-0">
                      {{item.ownerName}}
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      From
                    </v-card-text>
                    <v-card-text class="px-0">
                      {{item.doctorFirstname}}
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card class='custom_card'>
                    <v-card-text class="px-0 text_grey">
                      Use Date
                    </v-card-text>
                    <v-card-text class="px-0">
                      {{item.date}}
                    </v-card-text>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
            <v-layout row justify-center>
              <v-dialog v-model="dialog" fullscreen transition="dialog-bottom-transition" :overlay="false">
                <v-btn color="primary" dark slot="activator" @click='selected_request=item; getDetail()'>Detail</v-btn>
                <v-toolbar dark color="primary">
                  <v-btn icon @click.native="dialog = false" dark>
                    <v-icon>close</v-icon>
                  </v-btn>
                  <v-toolbar-title>Blood Request:{{selected_request.request_id}}</v-toolbar-title>
                </v-toolbar>
                <v-card>
                  <v-card-text>
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
                    <v-btn block class='bg__mdteal'  dark >ถุงเลือดที่ใช้</v-btn>
                    <v-card>
                      <v-container grid-list-md text-xs-center>
                        <v-layout row wrap v-for='item in selected_bags.detail' style='border-bottom: 1px solid #f4f4f4'>
                          <v-flex xs3>
                            <v-card class='custom_card'>
                              <v-card-text class="px-0 text_grey">
                                <img src='../assets/request/bloodbag.svg' style='width:40px'>
                                </img>
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.required_quantity}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs3>
                            <v-card class='custom_card'>
                              <v-card-text class="px-0 text_grey">
                                เลขที่การขอใช้เลือด
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.product_request_id}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs3>
                            <v-card class='custom_card'>
                              <v-card-text class="px-0 text_grey">
                                Type
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.product_type}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs3>
                            <v-card class='custom_card'>
                              <v-card-text class="px-0 text_grey">
                                สถานะ
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.request_status
                                }}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                        </v-layout>
                      </v-container>
        					  </v-card>
                    <v-btn block class='bg__mdteal'  dark >บันทึกผลการใช้เลือด</v-btn>
                    <v-card>
                      <v-container grid-list-md text-xs-center>
                        <v-layout row>
                          <v-flex xs3>
                            <v-subheader>บันทึกผลโดยย่อ</v-subheader>
                          </v-flex>
                          <v-flex xs6>
                            <v-text-field
                              v-model="comment"
                              label="ความคิดเห็นจากสัตวแพทย์"
                            ></v-text-field>
                          </v-flex>
                          <v-flex xs3>
                            <v-btn
                              @click="saveNote(selected_request.request_id)"
                            >
                            บันทึก
                            </v-btn>
                          </v-flex>
                        </v-layout>
                        <v-divider></v-divider>
                        <br>
                        <v-layout row wrap v-for='item in notes.diagnostic_result' style='border-bottom: 1px solid #f4f4f4'>
                          <v-flex xs1>
                            <v-card class='custom_card2'>
                              <v-card-text class="px-0 text_grey">
                                เลขที่บันทึก
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.diagnostic_id}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs3>
                            <v-card class='custom_card2'>
                              <v-card-text class="px-0 text_grey">
                                สัตวแพทย์ที่ตรวจ
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.doctor_firstname}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs3>
                            <v-card class='custom_card2'>
                              <v-card-text class="px-0 text_grey">
                                โรงพยาบาล
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.hospital_name}}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex xs5>
                            <v-card class='custom_card2'>
                              <v-card-text class="px-0 text_grey">
                                ความคิดเห็น
                              </v-card-text>
                              <v-card-text class="px-0">
                                {{item.others
                                }}
                              </v-card-text>
                            </v-card>
                          </v-flex>
                        </v-layout>
                      </v-container>
        					  </v-card>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-layout>
          </v-card-actions>
        </v-card>

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
    created () {
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/blood-bank/request/detail/request-byhospital', {
          hospitalID : "1"
        },headers)
        .then(response => {
					this.requests = response.data
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
    },
    components: {
      AppFooter
    },
    data () {
      return {
        requests: [],
        postBody: [],
        errors: [],
        current_pet: [],
        current_pet_detail: [],
        current_pet_owner: [],
        selected_request: [],
        selected_bags: [],
        notes: [],
        comment: '',
        dialog: false,
        clipped: false,
        drawer: false,
        clipped: false,
				breadcrumbs: [
          {
            text: 'ธนาคารเลือดสัตว์เลี้ยง',
            disabled: false
          },
          {
            text: 'การเบิกเลือด',
            disabled: true
          }
        ],
				items: [
					{ title: 'Dashboard', icon: 'dashboard', page: '/'},
          { title: 'Do it now!', icon: 'extension', page: '/'},
          { title: 'Statistic', icon: 'trending_up', page: '/'},
          { title: 'Email', icon: 'email', page: '/'},
          { title: 'Social', icon: 'message', page: '/'}
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
        show: false,
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
      route (page) {
        window.location.href = page
      },
      saveNote (id) {
        var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/diagnosticresult/add', {
          "vetID": "1",
          "hospitalID": "1",
          "PCV": "11",
          "others": this.comment,
          "resultID": id
        },headers)
        .then(response => {
					window.location.reload()
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

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

        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/diagnosticresult/getbyrequestid', {
          requestID : this.selected_request.request_id
        },headers)
        .then(response => {
					this.notes = response.data
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    },
    },
  }
</script>


<style lang="scss">
  $grey_line: #979797;

  .text_grey{
    color: $grey_line;
  }
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

  .stat_task{
    font-size: 3em;
    font-weight: bold;
  }
  .caption_task{
    font-size: 1.4em;
    font-weight: bold;
  }
  .custom_card{
    box-shadow: 0px 0px 0px 0px !important;
    border-right: 1px solid $grey_line;
  }
  .custom_card2{
    box-shadow: 0px 0px 0px 0px !important;
  }

</style>
