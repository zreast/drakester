<template>
  <v-app light id="inspire">
    <v-toolbar app class="navbar white--text">
      <v-avatar>
      <img src="/static/drake.png" alt="Drake">
      </v-avatar>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn outline class="white--text">
        About
      </v-btn>
      <v-btn icon class="white--text">
        <v-icon>refresh</v-icon>
      </v-btn>
      <v-btn icon class="white--text">
        <v-icon>more_vert</v-icon>
      </v-btn>
    </v-toolbar>

		<v-content style='background: linear-gradient(#1867c0, #9198e5); !important;'>
      <v-container fluid style='max-width:900px; display:block; margin:0 auto;'>
        <v-jumbotron color="grey lighten-2" v-show='state=="init"'>
          <v-container fill-height>
            <v-layout align-center>
              <v-flex class='white--text' xs4>
                <img src="/static/drakedrake.png" alt="Drake" style='width:250px'>
              </v-flex>
              <v-flex class='white--text' xs8>
                <h3 class="display-3">Drake's Spam Message Detector</h3>
                <br>
                <span class="subheading">Web appฯ อย่างง่าย สำหรับคนว่างมากๆ ตรวจว่าข้อความที่ได้รับมานั้นเป็นสแปมฤๅไม่ ด้วยเทคโนโลยี 4.0 แห่งอนาคตปี3018 กับโมเดลสุดอัจฉริยะ ผ่านการทดลองและทดสอบมาอย่างดีในนาม <b>'Drakester'</b> จะเป็นผู้ตัดสินชี้ชะตาให้.</span>
                <v-divider class="my-3"></v-divider>
                <div class="title mb-3">Dev with ❤ by Zreast. <a href='http://zreast.me' style='color:white; font-style:italic' target='blank'>http://zreast.me</a></div>
              </v-flex>
            </v-layout>
          </v-container>
        </v-jumbotron>
        <v-jumbotron color="grey lighten-2" v-show='state=="spam"'>
          <v-container fill-height>
            <v-layout align-center>
              <v-flex class='white--text' xs4>
                <img src="/static/spam.jpg" alt="Drake" style='width:250px; border-radius:100%' class='animated bounceIn' v-show='!waiting'>
                <img src="/static/spam2.jpg" alt="Drake" style='width:250px; border-radius:100%' v-show='waiting'>
              </v-flex>
              <v-flex class='white--text' xs8>
                <h3 class="display-3">SPAM</h3>
                <br>
                <div class="title mb-3">Yes, it's you.</div>
              </v-flex>
            </v-layout>
          </v-container>
        </v-jumbotron>
        <v-jumbotron color="grey lighten-2" v-show='state=="ham"'>
          <v-container fill-height>
            <v-layout align-center>
              <v-flex class='white--text' xs4>
                <img src="/static/ham.jpg" alt="Drake" style='width:250px; border-radius:100%' class='animated bounceIn' v-show='!waiting'>
                <img src="/static/ham2.jpg" alt="Drake" style='width:250px; border-radius:100%' v-show='waiting'>
              </v-flex>
              <v-flex class='white--text' xs8>
                <h3 class="display-3">NO</h3>
                <br>
                <div class="title mb-3">safe</div>
              </v-flex>
            </v-layout>
          </v-container>
        </v-jumbotron>
        <div>
          <v-card>
            <v-container fluid>
              <v-layout row>
                <v-flex xs12>
                  <v-text-field
                    name="input-1"
                    label="Paste your message here ( ͡° ͜ʖ ͡°)"
                    textarea
                    v-model='stringy'
                  ></v-text-field>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card>
        </div>
        <br>
        <v-btn outline color="white" dark large  @click='spamCheck()' style='display:block; margin:0 auto; padding: 1em; border: 2px solid; font-weight:bold; font-size:2em; width:300px' v-show='!waiting'>SPAM Check !</v-btn>
        <v-btn outline color="white" dark large style='display:block; margin:0 auto; padding: 1em; border: 2px solid; font-weight:bold; font-size:2em; width:300px' v-show='waiting'><i class="fa fa-circle-o-notch fa-spin" style="font-size:24px"></i></v-btn>

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
        title: "Drakester",
        stringy: '',
        result: null,
        state: 'init',
        waiting: false,
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
      callAPI()
      {
        var headers = {
            'Content-Type': 'application/json'
        }
        axios.post('https://obkefe4coi.execute-api.ap-southeast-1.amazonaws.com/test/', {
          "text": this.stringy
        },headers)
        .then(response => {
					this.result = response.data
          if(this.result.result.result=='spam')
          {
            this.state='spam'
          }
          if(this.result.result.result=='ham')
          {
            this.state='ham'
          }
          this.waiting = false
				})
		    .catch(e => {
		      this.errors.push(e)
		    })
      },
      spamCheck () {
        this.waiting = true;
				setTimeout(this.callAPI, 1000)
	    },
    }
  }
</script>


<style lang="scss">
  .color__mdred {
    color: #1867c0 !important;
  }

  .bg__mdteal {
    background-color: #5c9ba4 !important;
  }

  .navbar {
    background-color: #1867c0 !important;
    color: white;
    box-shadow: 0px 0px !important;
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
