<template>
	<v-app light id="inspire">
    <v-container>
      <v-btn color="primary" @click="callAPI">Test API</v-btn>
      <br>
      {{posts}}
    </v-container>
  </v-app>
</template>

<script>
	/* eslint-disable */
  import axios from 'axios'

  export default {
    methods: {
	    callAPI () {
				// this.postBody.name = 8
				// axios.post(`https://myfuckingapi.herokuapp.com/api/results/hello-view/`, {
		    //   body: this.postBody
		    // })
		    // .then(response => {
				// 	this.posts = response.data
				// 	console.log(this.posts)
				// })
		    // .catch(e => {
		    //   this.errors.push(e)
		    // })
				var headers = {
            'Content-Type': 'application/json'
        }
			  axios.post('https://odnooein50.execute-api.ap-southeast-1.amazonaws.com/Dev/results', {
    "petID" : "8"
  },headers)
   .then(response => {
					this.posts = response.data
					console.log(this.posts)
				})
		    .catch(e => {
		      this.errors.push(e)
		    })


	    }
	  },
    data () {
      return {
				posts: [],
				postBody: [],
      	errors: []
      }
    }
  }
</script>


<style>

</style>
