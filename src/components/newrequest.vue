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
            </v-icon> Blood Request</h1>
            <br>

						<div class="progress" v-if='page=="profile"'>
							<a @click='page="profile";  componentUpdate()'>
							  <div class="circle active">
							    <span class="label"></span>
							    <span class="title">Profile</span>
							  </div>
							</a>
						  <span class="bar"></span>
							<a @click='page="lab";  componentUpdate()'>
							  <div class="circle">
							    <span class="label"></span>
							    <span class="title">Lab</span>
							  </div>
							</a>
						  <span class="bar"></span>
							<a @click='page="suggestion";  componentUpdate()'>
							  <div class="circle">
							    <span class="label"></span>
							    <span class="title">Request</span>
							  </div>
							</a>
              <span class="bar"></span>
							<a @click='page="confirm";  componentUpdate()'>
							  <div class="circle">
							    <span class="label"></span>
							    <span class="title">Confirm</span>
							  </div>
							</a>
						</div>
            <div class="progress" v-if='page=="lab"'>
							<a @click='page="profile";  componentUpdate()'>
							  <div class="circle done">
							    <span class="label"></span>
							    <span class="title">Profile</span>
							  </div>
							</a>
						  <span class="bar done"></span>
							<a @click='page="lab";  componentUpdate()'>
							  <div class="circle active">
							    <span class="label"></span>
							    <span class="title">Lab</span>
							  </div>
							</a>
						  <span class="bar half"></span>
							<a @click='page="suggestion";  componentUpdate()'>
								<div class="circle">
							    <span class="label"></span>
							    <span class="title">Request</span>
							  </div>
							</a>
              <span class="bar"></span>
							<a @click='page="confirm";  componentUpdate()'>
							  <div class="circle">
							    <span class="label"></span>
							    <span class="title">Confirm</span>
							  </div>
							</a>
						</div>
            <div class="progress" v-if='page=="suggestion"||page=="bloodbag"'>
							<a @click='page="profile";  componentUpdate()'>
							  <div class="circle done">
							    <span class="label"></span>
							    <span class="title">Profile</span>
							  </div>
							</a>
						  <span class="bar done"></span>
							<a @click='page="lab";  componentUpdate()'>
							  <div class="circle done">
							    <span class="label"></span>
							    <span class="title">Lab</span>
							  </div>
							</a>
						  <span class="bar done"></span>
							<a @click='page="suggestion";  componentUpdate()'>
								<div class="circle active">
							    <span class="label"></span>
							    <span class="title">Request</span>
							  </div>
							</a>
              <span class="bar"></span>
							<a @click='page="confirm";  componentUpdate()'>
							  <div class="circle">
							    <span class="label"></span>
							    <span class="title">Confirm</span>
							  </div>
							</a>
						</div>
            <div class="progress" v-if='page=="confirm"'>
							<a @click='page="profile";  componentUpdate()'>
							  <div class="circle done">
							    <span class="label"></span>
							    <span class="title">Profile</span>
							  </div>
							</a>
						  <span class="bar done"></span>
							<a @click='page="lab";  componentUpdate()'>
							  <div class="circle done">
							    <span class="label"></span>
							    <span class="title">Lab</span>
							  </div>
							</a>
						  <span class="bar done"></span>
							<a @click='page="suggestion";  componentUpdate()'>
								<div class="circle done">
							    <span class="label"></span>
							    <span class="title">Request</span>
							  </div>
							</a>
              <span class="bar done"></span>
							<a @click='page="confirm";  componentUpdate()'>
							  <div class="circle active">
							    <span class="label"></span>
							    <span class="title">Confirm</span>
							  </div>
							</a>
						</div>
						<br>
	          <v-btn block>Recipient Profile</v-btn>
						<v-card>
					    <v-card-title>
								<v-text-field
					        append-icon="search"
					        label="Pet Name, H.N., Owner, Address"
					        single-line
					        hide-details
					        v-model="petID"
                  @change="getPetID()"
                  @keyup.enter="getPetID()"
					      ></v-text-field>
								<v-btn
						      color="grey"
						      class="white--text"
						    >
									<v-icon dark>add</v-icon>
						      Add New Pet
						    </v-btn>
                <v-spacer v-show='current_pet_detail.petID==null'></v-spacer>
                <v-layout justify-center column style="margin-left: 2em" v-show='current_pet_detail.petID!=null'>
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
                              v-if="current_pet_detail.img"
                            >
                            <v-icon v-else>{{ current_pet_detail.img }}</v-icon>
                          </v-avatar>
                        </v-flex>
                        <v-flex xs3>
                          <v-chip
                            label
                            small
                            class="ml-0"
                            v-if="current_pet_detail.petName"
                          >{{current_pet_detail.patientSpecies}}</v-chip>
                          <br>
                          <h2 v-html="current_pet_detail.petName"/>
                          <span class="grey--text" v-if="current_pet_detail.petID">&nbsp;(ID: {{current_pet_detail.petID}})</span>
                        </v-flex>
                        <v-flex xs4>
                          <b>เพศ:</b> {{current_pet_detail.patientGender}}<br>
                          <b>วันเกิด:</b> {{current_pet_detail.patientBirthDt}}<br>
                        </v-flex>
                        <v-flex xs4>
                          <b>เจ้าของ:</b> {{current_pet_owner.ownerName}}<br>
                          <b>เบอร์ติดต่อ:</b> {{current_pet_owner.TelNumer}}<br>
                        </v-flex>
                      </v-layout>

                    </v-card-text>
                  </v-card>
                </v-layout>
					    </v-card-title>
						</v-card>
						<br>
	          <v-btn block class='bg__mdteal' dark v-show='page=="profile"'>Medical Record</v-btn>
						<v-container grid-list-md text-xs-center v-show='page=="profile"'>
					    <v-layout row wrap>
					      <v-flex xs6>
					        <v-card>
					          <v-btn block>Physical Exam</v-btn>
										<v-flex>
											<v-layout row>
					              <v-text-field
					                name="input-1"
					                label="Weight (kg.)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
												<v-text-field
					                name="input-1"
					                label="Temp (F)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
												<v-text-field
					                name="input-1"
					                label="HR (bmp)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
											</v-layout>
											<v-layout row>
					              <v-text-field
					                name="input-1"
					                label="RR (tpm)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
												<v-text-field
					                name="input-1"
					                label="CRT (Sec)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
												<v-text-field
					                name="input-1"
					                label="BP (mmHg)"
					                textarea
													rows=3
													class='pa-2'
					              ></v-text-field>
											</v-layout>
				            </v-flex>
					        </v-card>
					      </v-flex>
								<v-flex xs6>
									<v-flex text-xs-left>
						        <v-card>
						          <v-btn block>History Taking</v-btn>
											<v-card class='pa-2'>
												เคยได้รับเลือดมาก่อน
												<v-layout row>
													<v-flex xs6>
														<v-radio-group :mandatory="false">
								              <v-radio label="ไม่เคย" value="radio-1"></v-radio>
								              <v-radio label="เคย เมื่อ" value="radio-2"></v-radio>
								            </v-radio-group>
													</v-flex>
													<v-flex xs6>
														<br><br>
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
											          @blur="date = parseDate(dateFormatted)"
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


											</v-card>
						        </v-card>
						      </v-flex>

									<v-flex text-xs-left>
						        <v-card>
						          <v-btn block>Diagnosis</v-btn>
											<v-card class='pa-2'>
												สาเหตุของความต้องการเลือด / Diagnosis
                        <v-select
                          :items="diagnosis"
                          v-model="post_diagnosis"
                          label="Select"
                          single-line
                          bottom
                        ></v-select>

                        <v-radio-group v-if="post_diagnosis.text=='Anemia'" v-model="post_anemia" :mandatory="false">
                          <v-radio label="regenerative" value="case1"></v-radio>
                          <v-radio label="nonregenerative" value="case2"></v-radio>
                          <v-radio label="with hypoproteinemia" value="case3"></v-radio>
                          <v-radio label="with hypovolemia" value="case4"></v-radio>
                          <v-radio label="with coagulopathy" value="case5"></v-radio>
                        </v-radio-group>
                        <v-select
                          v-if="post_diagnosis.text=='อื่นๆ'"
                          :items="post_disease"
                          v-model="post_other"
                          label="ระบุ"
                          autocomplete
                        ></v-select>
											</v-card>
						        </v-card>
						      </v-flex>

									<v-flex text-xs-left>
						        <v-card>
						          <v-btn block>Date Use</v-btn>
											<v-card class='pa-2'>
												วันที่ต้องการใช้เลือด
												<v-layout row>
													<v-flex xs6>
														<v-radio-group :mandatory="false">
								              <v-radio label="ทันที" value="radio-1"></v-radio>
								              <v-radio label="จองใช้เมื่อ" value="radio-2"></v-radio>
								            </v-radio-group>
													</v-flex>
													<v-flex xs6>
														<br><br>
														<v-menu
											        lazy
											        :close-on-content-click="false"
											        v-model="menu"
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
											          v-model="dateFormatted"
											          prepend-icon="event"
											          @blur="date = parseDate(dateFormatted)"
											        ></v-text-field>
											        <v-date-picker v-model="date" @input="dateFormatted = formatDate($event)" no-title scrollable actions>
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


											</v-card>
						        </v-card>
						      </v-flex>

									<v-flex text-xs-left>
						        <v-card>
						          <v-btn block>Note</v-btn>
											<v-card class='pa-2'>
												ความคิดเห็นอื่นๆ
												<v-flex>
							            <v-text-field
							              name="vet_comment"
							              label="บันทึกเพิ่มเติม"
							              multi-line
							            ></v-text-field>
							          </v-flex>
											</v-card>
						        </v-card>
						      </v-flex>
					      </v-flex>
					    </v-layout>
					  </v-container>
            <v-btn block class='bg__mdteal' dark v-show='page=="lab"'>Lab Exam</v-btn>

            <v-card v-show='page=="lab"' class='bg__grey'>
					    <v-card-title>
					      <h2>Complete Blood Count (CBC)</h2>
					    </v-card-title>

              <v-layout row wrap>
                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="HCT" v-model="hct" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="PLT" v-model="plt" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>

                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="HbG" v-model="hgb" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="RBC (M/usl)" v-model="rbc" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>

                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="MCH (M/usl)" v-model="mch" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>
                <v-flex xs3 class='result_box'>
                  <v-text-field box label="WBC" v-model="wbc" type="number"></v-text-field>
                </v-flex>
                <v-flex xs2 class='result_box'>
                </v-flex>


              </v-layout>
            </v-card>
            <br>
            <v-card v-show='page=="lab"'>
					    <v-card-title>
					      <h2>Blood Chemistry</h2>
					      <v-spacer></v-spacer>
								<v-btn
						      color="grey"
						      class="white--text"
									@click="getLatestExam()"
						    >
						      ล่าสุด
						      <v-icon right dark>update</v-icon>
						    </v-btn>
								<v-btn
						      color="grey"
						      class="white--text"
                  @click="show_all_exam=true; show_recent_exam=false; show_spec_exam=false;"
						    >
						      ทั้งหมด
						      <v-icon right dark>sort</v-icon>
						    </v-btn>
					    </v-card-title>

              <v-btn round dark class='bg__mdteal' @click='show_all_exam=true; show_spec_exam=false; show_recent_exam=false;' v-show='show_spec_exam==true'>
                <v-icon dark>arrow_back</v-icon>
                &nbsp;Back
              </v-btn>
              <v-data-table
					        v-bind:headers="result_headers"
					        v-bind:items="t_items"
					        v-bind:search="search"
                  v-show='show_spec_exam==true'
					      >
					      <template slot="items" slot-scope="props">
					        <td>
					          <v-edit-dialog
					            lazy
					          > {{ props.item.name }}
					            <v-text-field
					              slot="input"
					              label="Edit"
					              v-model="props.item.name"
					              single-line
					              counter
					              :rules="[max25chars]"
					            ></v-text-field>
					          </v-edit-dialog>
					        </td>
					        <td class="text-xs-right">{{ props.item.result_value }}</td>
					        <td class="text-xs-right">{{ props.item.result_value_uom_cd }}</td>
					        <td class="text-xs-right">{{ props.item.critical_low }}</td>
					        <td class="text-xs-right">{{ props.item.low }}</td>
					        <td class="text-xs-right">{{ props.item.high }}</td>
					        <td class="text-xs-right">{{ props.item.critical_high }}</td>
					        <td class="text-xs-right">
					          <v-edit-dialog
					            @open="tmp = props.item.iron"
					            @save="props.item.iron = tmp || props.item.iron"
					            large
					            lazy
					          >
					            <div>{{ props.item.iron }}</div>
					            <div slot="input" class="mt-3 title">Update Iron</div>
					            <v-text-field
					              slot="input"
					              label="Edit"
					              v-model="tmp"
					              single-line
					              counter
					              autofocus
					              :rules="[max25chars]"
					            ></v-text-field>
					          </v-edit-dialog>
					        </td>
					      </template>
					      <template slot="pageText" slot-scope="{ pageStart, pageStop }">
					        From {{ pageStart }} to {{ pageStop }}
					      </template>
					    </v-data-table>

              <v-data-table
					        v-bind:headers="result_headers"
					        v-bind:items="t_items"
					        v-bind:search="search"
                  v-show='show_recent_exam==true'
					      >
					      <template slot="items" slot-scope="props">
					        <td>
					          <v-edit-dialog
					            lazy
					          > {{ props.item.name }}
					            <v-text-field
					              slot="input"
					              label="Edit"
					              v-model="props.item.name"
					              single-line
					              counter
					              :rules="[max25chars]"
					            ></v-text-field>
					          </v-edit-dialog>
					        </td>
					        <td class="text-xs-right">{{ props.item.result_value }}</td>
					        <td class="text-xs-right">{{ props.item.result_value_uom_cd }}</td>
					        <td class="text-xs-right">{{ props.item.critical_low }}</td>
					        <td class="text-xs-right">{{ props.item.low }}</td>
					        <td class="text-xs-right">{{ props.item.high }}</td>
					        <td class="text-xs-right">{{ props.item.critical_high }}</td>
					        <td class="text-xs-right">
					          <v-edit-dialog
					            @open="tmp = props.item.iron"
					            @save="props.item.iron = tmp || props.item.iron"
					            large
					            lazy
					          >
					            <div>{{ props.item.iron }}</div>
					            <div slot="input" class="mt-3 title">Update Iron</div>
					            <v-text-field
					              slot="input"
					              label="Edit"
					              v-model="tmp"
					              single-line
					              counter
					              autofocus
					              :rules="[max25chars]"
					            ></v-text-field>
					          </v-edit-dialog>
					        </td>
					      </template>
					      <template slot="pageText" slot-scope="{ pageStart, pageStop }">
					        From {{ pageStart }} to {{ pageStop }}
					      </template>
					    </v-data-table>

              <v-data-table
                :headers="headers"
                v-bind:items="current_pet.results"
                v-bind:search="search"
                v-show='show_all_exam==true'
                hide-actions
                class="elevation-1"
              >
                <template slot="items" slot-scope="props">
                  <td class="text-xs-right">{{ props.item.result_id }}</td>
					        <td class="text-xs-right">{{ props.item.date }}</td>
					        <td class="text-xs-right">{{ props.item.doctor_id }}</td>
					        <td class="text-xs-right">{{ props.item.doctor_firstname }}</td>
					        <td class="text-xs-right">{{ props.item.doctor_lastname }}</td>
                  <td class="justify-center layout px-0">
                    <v-btn icon class="mx-0" @click="getSpecExam(props.item.result_id)">
                      <v-icon color="teal">remove_red_eye</v-icon>
                    </v-btn>
                  </td>
                </template>
              </v-data-table>
					  </v-card>
            <v-btn block class='bg__mdteal'  dark v-show='page=="suggestion"||page=="bloodbag"'>เป้าหมายของการให้เลือด</v-btn>
						<v-card v-show='page=="suggestion"||page=="bloodbag"'>
							<v-container fluid>
        				<v-layout row>
									<v-flex xs4>
				          </v-flex>
									<v-flex xs4>
                    <span v-show='recommended_products!=null' style='font-size:1.2em'>ผลิตภัณฑ์เลือดที่แนะนำ : </span>
                    <span v-show='recommended_products!=null' style='font-size:1.2em; font-weight: bold; color: #2196f3'>{{recommended_products}}</span>
										<v-layout row v-show='pcv'>
											<h2 style='padding-top:1em'>Target PCV</h2>
                      <v-text-field
      					        label="Type Here"
      					        single-line
      					        hide-details
      					        v-model="t_pcv"
                        type="number"
                        style="margin-left:2em"
      					      ></v-text-field>
                      <v-icon style='padding-top:0.5em; cursor: pointer' @click='pcv=false'>cancel</v-icon>
										</v-layout>
                    <v-layout row v-show='pp'>
											<h2 style='padding-top:1em'>Target PP</h2>
                      <v-text-field
      					        label="Type Here"
      					        single-line
      					        hide-details
      					        v-model="t_pp"
                        type="number"
                        style="margin-left:2em"
      					      ></v-text-field>
                      <v-icon style='padding-top:0.5em; cursor: pointer' @click='pp=false'>cancel</v-icon>
										</v-layout>
                    <v-layout row v-show='plt'>
											<h2 style='padding-top:1em'>Target PLT</h2>
                      <v-text-field
      					        label="Type Here"
      					        single-line
      					        hide-details
      					        v-model="t_plt"
                        type="number"
                        style="margin-left:2em"
      					      ></v-text-field>
                      <v-icon style='padding-top:0.5em; cursor: pointer' @click='plt=false'>cancel</v-icon>
										</v-layout>
                    <v-layout row v-show='alb'>
											<h2 style='padding-top:1em'>Target Alb</h2>
                      <v-text-field
      					        label="Type Here"
      					        single-line
      					        hide-details
      					        v-model="t_alb"
                        type="number"
                        style="margin-left:2em"
      					      ></v-text-field>
                      <v-icon style='padding-top:0.5em; cursor: pointer' @click='alb=false'>cancel</v-icon>
										</v-layout>
                    <br>

                    <v-dialog v-model="dialog" scrollable max-width="300px">
                      <v-btn fab dark style='max-width:30px; max-height:30px' slot="activator">
                        <v-icon dark>add</v-icon>
                      </v-btn>
                      <b slot="activator">เพิ่มเป้าหมายการให้เลือด</b>
                      <v-card>
                        <v-card-title>เพิ่มเป้าหมายการให้เลือด</v-card-title>
                        <v-divider></v-divider>
                        <v-card-text style="height: 300px;">
                          <v-btn block v-show='pcv==false' @click='pcv=true; dialog = false;'>PCV</v-btn>
                          <v-btn block v-show='pp==false' @click='pp=true; dialog = false;'>PP</v-btn>
                          <v-btn block v-show='plt==false' @click='plt=true; dialog = false;'>PLT</v-btn>
                          <v-btn block v-show='alb==false' @click='alb=true; dialog = false;'>Alb</v-btn>
                        </v-card-text>
                      </v-card>
                    </v-dialog>

                    <br>
                    <div class="text-xs-center" v-show='page=="profile"'>
        	           <v-btn color="error" dark large @click='page="lab"; componentUpdate()'>Next</v-btn>
        		        </div>
			            </v-flex>
								</v-layout>
							</v-container>
					  </v-card>
						<v-btn block class='bg__mdteal'  dark v-show='page=="bloodbag"'>ผลิตภัณฑ์เลือดที่ต้องการ</v-btn>
            <v-card style='margin-bottom: 1em' v-show='page=="bloodbag"' >
              <h2 style='margin:2em 2em 0em 2em'>ภายในหน่วยงาน</h2>
              <v-card-actions>
                <v-container grid-list-md text-xs-center>
                  <v-layout row wrap v-for='item in in_bloodbags' style='border-bottom: 1px solid #f4f4f4'>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          <img src='../assets/request/bloodbag.svg' style='width:40px'>
                          </img>
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.quantity}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          เลขที่ถุงเลือด
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.bag_id}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          Type
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.product_type}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          PCV
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.pcv}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          วันหมดอายุ
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.ExpDate}}
                        </v-card-text>
                      </v-card>
                    </v-flex>

                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0">
                          <v-btn @click='selected_bloodbags=item; page="confirm"'>เลือกถุงเลือด</v-btn>
                        </v-card-text>
                      </v-card>
                    </v-flex>

                  </v-layout>
                </v-container>

              </v-card-actions>

              <h2 style='margin:2em 2em 0em 2em'>ภายนอกหน่วยงาน</h2>
              <v-card-actions>
                <v-container grid-list-md text-xs-center>
                  <v-layout row wrap v-for='item in out_bloodbags' style='border-bottom: 1px solid #f4f4f4'>
                    <v-flex xs12>
                      <h3>{{item.hospital_name}}</h3>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          <img src='../assets/request/bloodbag.svg' style='width:40px'>
                          </img>
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.quantity}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          เลขที่ถุงเลือด
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.bag_id}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          Type
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.product_type}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          PCV
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.pcv}}
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0 text_grey">
                          วันหมดอายุ
                        </v-card-text>
                        <v-card-text class="px-0">
                          {{item.ExpDate}}
                        </v-card-text>
                      </v-card>
                    </v-flex>

                    <v-flex xs2>
                      <v-card class='custom_card'>
                        <v-card-text class="px-0">
                          <v-btn @click='selected_bloodbags=item; page="confirm"'>Contact</v-btn>
                        </v-card-text>
                      </v-card>
                    </v-flex>

                  </v-layout>
                </v-container>

              </v-card-actions>
            </v-card>

            <v-card v-show='page=="lab"' class='bg__grey'>
					    <v-card-title>
					      <h2>Test Kit</h2>
					    </v-card-title>

              <v-layout row wrap>
                <v-flex xs5>
                  <div style='margin-left: 2em'>
                    <v-layout>
                      <b>E.Canis/ E.ewingii Ab</b>
                    </v-layout>
                      <v-radio-group v-model="radios1" :mandatory="false">
                        <v-radio label="ไม่ได้ตรวจ" value="radio-1"></v-radio>
                        <v-radio label="Positive" value="radio-2"></v-radio>
                        <v-radio label="Negative" value="radio-3"></v-radio>
                      </v-radio-group>
                  </div>
                </v-flex>
                <v-flex xs5>
                  <div style='margin-left: 2em'>
                    <v-layout>
                      <b>FIV Ab</b>
                    </v-layout>
                      <v-radio-group v-model="radios2" :mandatory="false">
                        <v-radio label="ไม่ได้ตรวจ" value="radio-1"></v-radio>
                        <v-radio label="Positive" value="radio-2"></v-radio>
                        <v-radio label="Negative" value="radio-3"></v-radio>
                      </v-radio-group>
                  </div>
                </v-flex>
                <v-flex xs5>
                  <div style='margin-left: 2em'>
                    <v-layout>
                      <b>Lyme Disease</b>
                    </v-layout>
                      <v-radio-group v-model="radios3" :mandatory="false">
                        <v-radio label="ไม่ได้ตรวจ" value="radio-1"></v-radio>
                        <v-radio label="Positive" value="radio-2"></v-radio>
                        <v-radio label="Negative" value="radio-3"></v-radio>
                      </v-radio-group>
                  </div>
                </v-flex>
                <v-flex xs5>
                  <div style='margin-left: 2em'>
                    <v-layout>
                      <b>FeLV Ag</b>
                    </v-layout>
                      <v-radio-group v-model="radios4" :mandatory="false">
                        <v-radio label="ไม่ได้ตรวจ" value="radio-1"></v-radio>
                        <v-radio label="Positive" value="radio-2"></v-radio>
                        <v-radio label="Negative" value="radio-3"></v-radio>
                      </v-radio-group>
                  </div>
                </v-flex>
              </v-layout>


            </v-card>
            <v-btn block class='bg__mdteal'  dark v-show='page=="confirm"'>ยืนยันการรับถุงเลือด</v-btn>
            <v-card v-show='page=="confirm"'>
              <v-container grid-list-md text-xs-center>
                <v-layout row wrap style='border: 2px solid #e1e1e1; margin: 2em; border-radius: 10px;'>
                  <v-flex xs2>

                  </v-flex>
                  <v-flex xs2>
                    <v-card class='custom_card'>
                      <v-card-text class="px-0 text_grey">
                        เลขที่ถุงเลือด
                      </v-card-text>
                      <v-card-text class="px-0" style='font-size: 1.2em; font-weight: bold'>
                        {{selected_bloodbags.bag_id}}
                      </v-card-text>
                    </v-card>
                  </v-flex>
                  <v-flex xs2>
                    <v-card class='custom_card'>
                      <v-card-text class="px-0 text_grey">
                        PCV
                      </v-card-text>
                      <v-card-text class="px-0" style='font-size: 1.2em; font-weight: bold'>
                        {{selected_bloodbags.pcv}}
                      </v-card-text>
                    </v-card>
                  </v-flex>
                  <v-flex xs2>
                    <v-card class='custom_card'>
                      <v-card-text class="px-0 text_grey">
                        Type
                      </v-card-text>
                      <v-card-text class="px-0" style='font-size: 1.2em; font-weight: bold'>
                        {{selected_bloodbags.product_type}}
                      </v-card-text>
                    </v-card>
                  </v-flex>
                  <v-flex xs2>
                    <v-card class='custom_card'>
                      <v-card-text class="px-0 text_grey">
                        ปริมาณที่เบิก
                      </v-card-text>
                      <v-card-text class="px-0" style='font-size: 1.2em; font-weight: bold'>
                        {{selected_bloodbags.quantity}}
                      </v-card-text>
                    </v-card>
                  </v-flex>
                </v-layout>
              </v-container>
					  </v-card>
            <br>

						<div class="text-xs-center" v-show='page=="profile"'>
	           <v-btn color="error" dark large @click='page="lab";  componentUpdate()'>Next</v-btn>
		        </div>
            <div class="text-xs-center" v-show='page=="lab"'>
	           <v-btn color="error" dark large @click='page="suggestion";  componentUpdate()'>Next</v-btn>
		        </div>
            <div class="text-xs-center" v-show='page=="suggestion"'>
	           <v-btn color="info" dark large @click='page="bloodbag"; getBloodBag();  componentUpdate()'>เลือกถุงเลือดที่ต้องการ</v-btn>
		        </div>
            <div class="text-xs-center" v-show='page=="confirm"'>
	           <v-btn color="error" dark large @click='sendRequest()'>ยืนยันการรับถุงเลือด</v-btn>
		        </div>
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
        temp: [],
        current_pet: [],
        current_pet_detail: [],
        current_pet_owner: [],
        current_result: [],
        bloodbags: [],
        in_bloodbags: [],
        out_bloodbags: [],
        selected_bloodbags: [],
        req_id: null,
        prod_req_id: null,
        petID: '',
        postBody: '',
        errors: [],
        page: 'profile',
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
        today_date: null,
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
        show_all_exam: true,
        show_recent_exam: false,
        show_spec_exam: false,
        diagnosis: [
          { text: 'Anemia' },
          { text: 'ผ่าตัด' },
          { text: 'อื่นๆ' },
        ],
        max25chars: (v) => v.length <= 25 || 'Input too long!',
        tmp: '',
        search: '',
        pagination: {},
        headers: [
          { text: 'result_id', value: 'result_id' },
          { text: 'date', value: 'date' },
          { text: 'doctor_id', value: 'doctor_id' },
          { text: 'doctor_firstname', value: 'doctor_firstname' },
          { text: 'doctor_lastname', value: 'doctor_lastname' },
          { text: 'actions', value: 'actions' }
        ],
        result_headers: [
          {
            text: 'Test',
            align: 'left',
            sortable: false,
            value: 'name'
          },
          { text: 'value', value: 'value' },
          { text: 'value_oum_cd', value: 'value_oum_cd' },
          { text: 'critical low', value: 'critical_low' },
          { text: 'low', value: 'low' },
          { text: 'high', value: 'high' },
          { text: 'critical high', value: 'critical_high' }
        ],
        t_items: [],
        hct: null,
        hgb: null,
        rbc: null,
        mch: null,
        wbc: null,
        pcv: true,
        pp: false,
        plt: false,
        alb: false,
        t_pcv: null,
        t_alb: null,
        t_plt: null,
        t_pp: null,
        radios1: null,
        radios2: null,
        radios3: null,
        radios4: null,
        dialog: false,
        rec_wb: false,
        rec_prbc: false,
        rec_ffp: false,
        rec_fp: false,
        rec_cryo: false,
        rec_colloid: false,
        rec_plt: false,
        post_diagnosis: [],
        post_anemia: null,
        post_other: null,
        post_disease: [
          "von Willebrand's", 'Hypoproteinemia', 'Low immunoglobulin', 'Hemophelia A', 'Hemophelia B', 'Disseminated intravascular coagulopathy', 'Pancreatitis', 'Liver disease', 'Thrombocytopenia'
        ],
        recommended_products: null
      }
    },
		methods: {
      componentUpdate () {
        this.rec_wb = false
        this.rec_prbc = false
        this.rec_ffp = false
        this.rec_fp = false
        this.rec_cryo = false
        this.rec_colloid = false
        this.rec_plt = false
        this.recommended_products = null

        if(this.post_diagnosis.text=='Anemia')
        {
          //regenerative
          if(this.post_anemia=='case1')
          {
            this.rec_prbc = true;
          }
          //regenerative
          else if (this.post_anemia=='case2')
          {
            this.rec_prbc = true;
          }
          //with hypoproteinemia or with hypovolemia
          else if (this.post_anemia=='case3'||this.post_anemia=='case4')
          {
            this.rec_wb = true;
            this.rec_prbc = true;
            this.rec_fp = true;
            this.rec_colloid = true;
          }
          //with coagulopathy
          else if (this.post_anemia=='case5')
          {
            this.rec_wb = true;
            this.rec_prbc = true;
            this.rec_ffp = true;
          }
        }
        else if(this.post_diagnosis.text=='อื่นๆ')
        {
          if(this.post_other=="von Willebrand's")
          {
            this.rec_ffp = true;
            this.rec_cryo = true;
          }
          else if(this.post_other=="Hypoproteinemia")
          {
            this.rec_fp = true;
            this.rec_colloid = true;
          }
          else if(this.post_other=="Low immunoglobulin")
          {
            this.rec_ffp = true;
            this.rec_fp = true;
          }
          else if(this.post_other=="Hemophelia A")
          {
            this.rec_ffp = true;
            this.rec_cryo = true;
          }
          else if(this.post_other=="Hemophelia B")
          {
            this.rec_ffp = true;
            this.rec_fp = true;
          }
          else if(this.post_other=="Disseminated intravascular coagulopathy")
          {
            this.rec_wb = true;
            this.rec_ffp = true;
            this.rec_fp = true;
            this.rec_plt = true;
          }
          else if(this.post_other=="Pancreatitis")
          {
            this.rec_cryo = true;
          }
          else if(this.post_other=="Liver disease")
          {
            this.rec_fp = true;
          }
          else if(this.post_other=="Thrombocytopenia")
          {
            this.rec_wb = true;
            this.rec_plt = true;
          }
        }
        if(this.rec_wb)
        {
          this.pcv = true;
          if(this.recommended_products == null)
            this.recommended_products = "Whole Blood";
          else
            this.recommended_products += ", Whole Blood";
        }
        if(this.rec_prbc)
        {
          this.pcv = true;
          if(this.recommended_products == null)
            this.recommended_products = "Packed Red Blood Cells";
          else
            this.recommended_products += ", Packed Red Blood Cells";
        }
        if(this.rec_ffp)
        {
          if(this.recommended_products == null)
            this.recommended_products = "Fresh Frozen Plasma";
          else
            this.recommended_products += ", Fresh Frozen Plasma";
        }
        if(this.rec_fp)
        {
          if(this.recommended_products == null)
            this.recommended_products = "Frozenplasma / Cryoprecipitate poor plasma";
          else
            this.recommended_products += ", Frozenplasma / Cryoprecipitate poor plasma";
        }
        if(this.rec_cryo)
        {
          if(this.recommended_products == null)
            this.recommended_products = "Cryoprecipitate";
          else
            this.recommended_products += ", Cryoprecipitate";
        }
        if(this.rec_colloid)
        {
          if(this.recommended_products == null)
            this.recommended_products = "Colloid";
          else
            this.recommended_products += ", Colloid";
        }
        if(this.rec_plt)
        {
          this.plt = true;
          if(this.recommended_products == null)
            this.recommended_products = "Platelets";
          else
            this.recommended_products += ", Platelets";
        }
      },
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
      getDate() {
        var today = new Date();
        var dd = today.getDate();
        var mm = today.getMonth()+1; //January is 0!

        var yyyy = today.getFullYear();
        var hh = today.getHours();
        var mn = today.getMinutes();
        var ss = today.getSeconds();
        if(dd<10){
            dd='0'+dd;
        }
        if(mm<10){
            mm='0'+mm;
        }
        if(hh<10){
            hh='0'+hh;
        }
        if(mn<10){
            mn='0'+mn;
        }
        if(ss<10){
            ss='0'+ss;
        }
        var today = yyyy+'-'+mm+'-'+dd+' '+hh+':'+mm+':'+ss;

        return today
      },
      sendRequestDetail (){
        this.today_date = this.getDate()
        var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/request/bloodproductrequestdetail/add', {
          request_date: this.today_date,
          bloodtype: this.selected_bloodbags.blood_type,
          product_type: this.selected_bloodbags.product_type,
          required_quantity: this.selected_bloodbags.quantity,
          request_id: this.req_id,
          request_status: "1",
        },headers)
        .then(response => {
					this.prod_req_id = response.data.product_request_detail_id
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/request/bloodbagrequestdetail/add', {
          quantity: this.selected_bloodbags.quantity,
          method: "test",
          bag_id: this.selected_bloodbags.bag_id,
          product_request_detail_id: this.prod_req_id
        },headers)
        .then(response => {
					window.location.href = '/success'
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
      },
      sendRequest () {
        this.today_date = this.getDate()
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/request/bloodrequesthistory/add', {
          hospital_id: "1",
          request_reason: this.post_diagnosis,
          pet_id: this.current_pet_detail.petID,
          vet_id: "1",
          date: this.today_date
        },headers)
        .then(response => {
          this.req_id = response.data.request_id
          this.sendRequestDetail()
				})
		    .catch(e => {
		      this.errors.push(e)
		    })

	    },
      getOwner ()
      {
        var headers = {
            'Content-Type': 'application/json'
        }

        //owner-detail
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/owner/detail', {
          ownerID : this.current_pet_detail.ownerID
        },headers)
        .then(response => {
					this.current_pet_owner = response.data
          console.log(this.current_pet_owner)
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
      },
      getPetID () {
				var headers = {
            'Content-Type': 'application/json'
        }

        //getresult
        // axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/results', {
        //   petID : this.petID
        // },headers)
        // .then(response => {
				// 	this.current_pet = response.data
				// })
		    // .catch(e => {
		    //   this.errors.push(e)
		    // })

        //pet-detail
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/pet/datail', {
          petID : this.petID
        },headers)
        .then(response => {
					this.current_pet_detail = response.data
          this.current_pet_detail.img = 'https://www.what-dog.net/Images/faces2/scroll007.jpg'
          this.getOwner()
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
	    },
      createResult() {
        this.t_items = []
        if(this.current_result.results.alt!=null)
          {this.temp = this.current_result.results.alt
          this.temp.name = 'ALT'
          this.t_items.push(this.temp)}

        if(this.current_result.results.crea!=null)
          {this.temp = this.current_result.results.crea
          this.temp.name = 'CREA'
          this.t_items.push(this.temp)}

        if(this.current_result.results.bun!=null)
          {this.temp = this.current_result.results.bun
          this.temp.name = 'BUN'
          this.t_items.push(this.temp)}

        if(this.current_result.results.crea_bun!=null)
          {this.temp = this.current_result.results.crea_bun
          this.temp.name = 'BUN/CREA'
          this.t_items.push(this.temp)}

        if(this.current_result.results.alb!=null)
          {this.temp = this.current_result.results.alb
          this.temp.name = 'ALB'
          this.t_items.push(this.temp)}

        if(this.current_result.results.alb_glob!=null)
          {this.temp = this.current_result.results.alb_glob
          this.temp.name = 'ALB/GLOB'
          this.t_items.push(this.temp)}

        if(this.current_result.results.alkp!=null)
          {this.temp = this.current_result.results.alkp
          this.temp.name = 'ALKP'
          this.t_items.push(this.temp)}

        if(this.current_result.results.glob!=null)
          {this.temp = this.current_result.results.glob
          this.temp.name = 'GLOB'
          this.t_items.push(this.temp)}

        if(this.current_result.results.tp!=null)
          {this.temp = this.current_result.results.tp
          this.temp.name = 'TP'
          this.t_items.push(this.temp)}
      },
      getLatestExam () {
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/results/lastest', {
          petID: this.current_pet_detail.petID
        },headers)
        .then(response => {
					this.current_result = response.data
					this.createResult()
          this.show_spec_exam = false;
          this.show_recent_exam = true;
          this.show_all_exam = false;
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    },
      getSpecExam (result_id) {
				var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/results/specific', {
          resultID: result_id
        },headers)
        .then(response => {
					this.current_result = response.data
					this.createResult()
          this.show_spec_exam = true;
          this.show_recent_exam = false;
          this.show_all_exam = false;
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    },
      getSuggestion (bloodbags) {
        var headers = {
            'Content-Type': 'application/json'
        }
        var send_bags = []
        var temp = []

        for(var i in bloodbags)
        {
          temp.volume = bloodbags[i].quantity
          temp.pcv = bloodbags[i].pcv
          send_bags.push(temp)
        }
        console.log(send_bags)
        //model beta
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/model', {
          "Weight": 3.4,
          "PCV_patient": 10,
          "PCV_target": 23.6,
          "bloodbag": [
          {

                  "volume": 300,
                  "pcv": 30
          },
          {

                  "volume": 500,
                  "pcv": 50
          }
  ,
          {

                  "volume": 1000,
                  "pcv": 30
          }

      ]
        },headers)
        .then(response => {
          console.log(response.data)
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
	    },
      getBloodBag () {
        var headers = {
            'Content-Type': 'application/json'
        }
        //getAllBag
        axios.post('https://nqh48rassj.execute-api.ap-southeast-1.amazonaws.com/deploy/blood-bank/blood-bag/get-all', {

        },headers)
        .then(response => {
					this.bloodbags = response.data
          this.in_bloodbags = []
          this.out_bloodbags = []
          for(var i in this.bloodbags.bloodBags)
          {
            if(this.bloodbags.bloodBags[i].hospital_id=='1')
            {
              this.in_bloodbags.push(this.bloodbags.bloodBags[i])
            }
            else if(this.bloodbags.blood_bags[i].hospital_id!='1')
            {
              this.out_bloodbags.push(this.bloodbags.bloodBags[i])
            }
          }
          this.getSuggestion(this.in_bloodbags)
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    },
      route (page) {
        window.location.href = page
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

  /* Form Progress */

  .progress {
    margin: 20px auto;
    text-align: center;
    .circle {
      display: inline-block;
      background: #fff;
      width: 28px;
      height: 28px;
      border-radius: 40px;
      border: 1px solid #d5d5da;
    }
    .bar {
      display: inline-block;
      background: #fff;
      width: 28px;
      height: 28px;
      border-radius: 40px;
      border: 1px solid #d5d5da;
      position: relative;
      width: 200px;
      height: 6px;
      top: -33px;
      margin-left: -5px;
      margin-right: -5px;
      border-left: none;
      border-right: none;
      border-radius: 0;
    }
    .circle {
      .label {
        display: inline-block;
        width: 20px;
        height: 20px;
        line-height: 32px;
        border-radius: 32px;
        margin-top: 3px;
        color: #b5b5ba;
        font-size: 17px;
      }
      .title {
        color: #b5b5ba;
        font-size: 13px;
        line-height: 30px;
      }
    }
    .bar.done, .circle.done {
      background: #eee;
    }
    .bar.active {
      background: linear-gradient(to right, #EEE 40%, #FFF 60%);
    }
    .circle {
      &.done {
        .label {
          color: #FFF;
          background: #8bc435;
          box-shadow: inset 0 0 2px rgba(0, 0, 0, 0.2);
        }
        .title {
          color: #444;
        }
      }
      &.active {
        .label {
          color: #FFF;
          background: #0c95be;
          box-shadow: inset 0 0 2px rgba(0, 0, 0, 0.2);
        }
        .title {
          color: #0c95be;
        }
      }
    }
  }

  .bg__grey{
    background-color: #f5f5f5 !important;
  }
  .result_box{

  }

  $grey_line: #979797;

  .text_grey{
    color: $grey_line;
  }
  .custom_card{
    box-shadow: 0px 0px 0px 0px !important;
  }
</style>
