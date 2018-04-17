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

            <h1 style='color:#00897B'><v-icon color="teal darken-1">account_balance
            </v-icon> Inventory</h1>
            <br>
            <v-layout row wrap>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" persistent max-width="500px">
                <v-btn
                  large
                  color="blue"
                  class="white--text"
                  slot="activator"
                >
                  <v-icon dark>add</v-icon>
                  เพิ่มถุงเลือด
                </v-btn>
                <v-card>
                  <v-card-title>
                    <span class="headline">เพิ่มถุงเลือด</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container grid-list-md>
                      <v-layout wrap>
                        <v-flex xs12 sm6 md6>
                          <v-select
                            label="ผลิตภัณฑ์"
                            required
                            :items="['FWB', 'SWB', 'pRBCs', 'PRP', 'FFP', 'FP']"
                            v-model=bag_add.product
                          ></v-select>
                        </v-flex>
                        <v-flex xs12 sm6 md6>
                          <v-text-field label="ปริมาณ" hint="ตัวเลขเท่านั้น"
                          v-model=bag_add.qt
                          required></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                          <v-text-field label="ค่า PCV"
                          v-model=bag_add.pcv required></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                          <v-text-field label="หมายเลขการบริจาคเลือด"
                          v-model=bag_add.donationID required></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                          <v-text-field label="หมายเหตุ"
                          v-model=bag_add.comment></v-text-field>
                        </v-flex>
                        <v-flex xs12 sm6>
                          <v-select
                            label="กรุ๊ปเลือด"
                            required
                            :items="['DEA1.1', 'DEA1.2', 'DEA3', 'DEA4', 'DEA5', 'DEA6', 'DEA7', 'DEA8']"
                            v-model=bag_add.dea
                          ></v-select>
                        </v-flex>
                        <v-flex xs12 sm6>
                          <v-menu
                            lazy
                            :close-on-content-click="false"
                            v-model="menu2"
                            transition="scale-transition"
                            offset-y
                            full-width
                            :nudge-right="40"
                            max-width="290px"
                            min-width="290px"
                          >
                            <v-text-field
                              slot="activator"
                              label="Date in M/D/Y"
                              v-model="dateFormatted2"
                              prepend-icon="event"
                              @blur="date = parseDate(dateFormatted2)"
                            ></v-text-field>
                            <v-date-picker v-model="date" @input="dateFormatted2 = formatDate($event)" no-title scrollable actions>
                              <template slot-scope="{ save, cancel }">
                                <v-card-actions>
                                  <v-spacer></v-spacer>
                                  <v-btn flat color="primary" @click="cancel">Cancel</v-btn>
                                  <v-btn flat color="primary" @click="save">OK</v-btn>
                                </v-card-actions>
                              </template>
                            </v-date-picker>
                          </v-menu>
                        </v-flex>
                      </v-layout>
                    </v-container>
                    <small>*indicates required field</small>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" flat @click.native="dialog = false">Close</v-btn>
                    <v-btn color="blue darken-1" flat @click.native="addBloodBag(); dialog = false">Save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-layout>
            <br><br>

            <v-container grid-list-md text-xs-center>
              <v-layout row wrap>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">FWB</v-card-text>
                    <span class='stat_task'>{{c_fwb}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_fwb}}</v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">SWB</v-card-text>
                    <span class='stat_task'>{{c_swb}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_swb}}</v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">pRBCs</v-card-text>
                    <span class='stat_task'>{{c_prbc}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_prbc}}</v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">PRP</v-card-text>
                    <span class='stat_task'>{{c_prp}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_prp}}</v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">FFP</v-card-text>
                    <span class='stat_task'>{{c_ffp}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_ffp}}</v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs2>
                  <v-card dark color="secondary">
                    <v-card-text class="px-0 caption_task">FP</v-card-text>
                    <span class='stat_task'>{{c_fp}}</span>
                    <v-card-text class="px-0 caption_task">จอง:{{r_fp}}</v-card-text>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>

          </v-layout>
        </v-slide-y-transition>
        <br>
        <v-alert v-show='deletetoggle' outline color="warning" icon="priority_high" :value="true" class="animated slideInUp">
          <div style='color:black'>
            ต้องการลบถุงเลือด <b>ID:{{del_id}} {{del_type}} ปริมาณ {{del_qt}} ml.</b> หรือไม่?
            <v-chip color="yellow darken-3" text-color="white" @click='deleteBloodBag(del_id)'>ตกลง</v-chip>
            <v-chip color="grey" text-color="white" @click='deletetoggle=false'>ยกเลิก</v-chip>
          </div>
        </v-alert>
        <br>
        <div>
          <v-data-table
            :headers="bloodbag_headers"
            :items="bloodbag_items.bloodBags"
            :search="search"
            :pagination.sync="pagination"
            hide-actions
            class="elevation-1"
          >
            <template slot="headerCell" slot-scope="props">
              <v-tooltip bottom>
                <span slot="activator">
                  {{ props.header.text }}
                </span>
                <span>
                  {{ props.header.text }}
                </span>
              </v-tooltip>
            </template>
            <template slot="items" slot-scope="props">
              <td>
                <v-chip v-show='props.item.bag_status!="active"' color="yellow darken-2" text-color="white">{{ props.item.bag_status }}</v-chip>
                <v-chip v-show='props.item.bag_status=="active"' color="green" text-color="white">{{ props.item.bag_status }}</v-chip>
              </td>
              <td>{{ props.item.quantity }}</td>
              <td>{{ props.item.bag_id }}</td>
              <td>{{ props.item.product_type }}</td>
              <td>{{ props.item.ExpDate }}</td>
              <td>{{ props.item.blood_type }}</td>
              <td>{{ props.item.pcv }}</td>
              <td>{{ props.item.vet_comment }}</td>
              <td class="text-xs-right">
                <v-icon style='cursor:pointer' @click='deletetoggle=true; del_id=props.item.bag_id; del_qt=props.item.quantity; del_type=props.item.product_type'>delete</v-icon>
              </td>
            </template>
          </v-data-table>
          <div class="text-xs-center pt-2">
            <v-pagination v-model="pagination.page" :length="pages"></v-pagination>
          </div>
        </div>
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
        this.bag_add.comment=' '
        this.bag_add.donationID='1'
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/blood-bank/blood-bag/get-by-hospitalid', {
          hospitalID : "1"
        },headers)
        .then(response => {
					this.bloodbag_items = response.data
          this.checkStatus()
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
        dateFormatted2: null,
	      menu: false,
				date2: null,
	      dateFormatted2: null,
	      menu2: false,
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Smart Pet Blood Bank',
        show: false,
        bloodbag_items: [],
        search: '',
        pagination: {},
        selected: [],
        bloodbag_headers: [
          { text: 'Status',align: 'left', value: 'bag_status' },
          {
            text: 'Volume (ml.)',
            align: 'left',
            value: 'quantity'
          },
          { text: 'ID', align: 'left',value: 'bag_id' },
          { text: 'Product Type', align: 'left',value: 'product_type' },
          { text: 'วันหมดอายุ', align: 'left',value: 'ExpDate' },
          { text: 'กรุ๊ปเลือด', align: 'left',value: 'blood_type' },
          { text: 'PCV', align: 'left',value: 'pcv' },
          { text: 'หมายเหตุ', align: 'left',value: 'vet_comment' },
          { text: 'Action' }
        ],
        dialog: false,
        deletetoggle: false,
        del_id: '',
        del_qt: '',
        del_type: '',
        c_fwb: 0,
        c_swb: 0,
        c_prbc: 0,
        c_prp: 0,
        c_ffp: 0,
        c_fp: 0,
        r_fwb: 0,
        r_swb: 0,
        r_prbc: 0,
        r_prp: 0,
        r_ffp: 0,
        r_fp: 0,
        bag_add: []
      }
    },
    computed: {
      pages () {
        if (this.pagination.rowsPerPage == null ||
          this.pagination.totalItems == null
        ) return 0

        return Math.ceil(this.pagination.totalItems / this.pagination.rowsPerPage)
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
      deleteBloodBag (id) {
        var headers = {
            'Content-Type': 'application/json'
        }
        //delete-bloodbag
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/blood-bank/blood-bag/delete', {
          bagID: id,
        },headers)
        .then(response => {
					window.location.reload()
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

      },
      checkStatus(){
        this.c_fwb = 0
        this.c_swb = 0
        this.c_prbc = 0
        this.c_prp = 0
        this.c_ffp = 0
        this.c_fp = 0
        this.r_fwb = 0
        this.r_swb = 0
        this.r_prbc = 0
        this.r_prp = 0
        this.r_ffp = 0
        this.r_fp = 0

        for(var i in this.bloodbag_items.bloodBags)
        {
          if(this.bloodbag_items.bloodBags[i].product_type=='FWB')
            {
              this.c_fwb++;
              if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
                this.r_fwb++;
            }
          if(this.bloodbag_items.bloodBags[i].product_type=='SWB'){
            this.c_swb++;
            if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
              this.r_swb++;
          }
          if(this.bloodbag_items.bloodBags[i].product_type=='pRBCs'){
            this.c_prbc++;
            if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
              this.r_prbc++;
          }
          if(this.bloodbag_items.bloodBags[i].product_type=='PRP'){
            this.c_prp++;
            if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
              this.r_prp++;
          }
          if(this.bloodbag_items.bloodBags[i].product_type=='FFP'){
            this.c_ffp++;
            if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
              this.r_ffp++;
          }
          if(this.bloodbag_items.bloodBags[i].product_type=='FP'){
            this.c_fp++;
            if(this.bloodbag_items.bloodBags[i].bag_status=='reserved')
              this.r_fp++;
          }
        }
      },
      addBloodBag () {
        var headers = {
            'Content-Type': 'application/json'
        }
        //bloodbag-add
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/blood-bank/blood-bag/add', {
          "bagDate": "2018-08-08",
          "donationID": this.bag_add.donationID,
          "type": "value",
          "expDate": "2018-08-08",
          "bagStatus": "active",
          "bloodType": this.bag_add.dea,
          "productType": this.bag_add.product,
          "hospitalID": "1",
          "quantity": this.bag_add.qt,
          "pcv": this.bag_add.pcv,
          "vetComment": this.bag_add.comment
        },headers)
        .then(response => {
          console.log(response.data)
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
