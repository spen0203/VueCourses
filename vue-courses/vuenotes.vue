<!-- DATA BINDING -->
	<!-- One way data bind shorthanded, allows the value to be populated by code only once -->
    <input type="email" :value="email" class="form-control form-control-lg">
					
	<!--Two way bind, Can be used to update values on the fly client or code side making error checking effecient -->
	<input type="email" v-model="email" class="form-control form-control-lg">

	<!-- CSS Binding, best used to conditionally style elements -->
		<!-- binded css will overide regular style tags -->
		<!-- Applys css to the element, -->
	<tr v-for="(a, index) in asteroids" :key="a"
							:style="{border:'solid 3px red'}" > 
		<!-- names that contain '-' must be '' or camel case  --> 
	<tr v-for="(a, index) in asteroids" :key="a"
							:style="{'border-top':'solid 3px red'}" > 
		<!-- Can be used with methods to return the css conditionally -->
	<tr v-for="(a, index) in asteroids" :key="a"
							:style="getRowStyle(a)" style="color: green" >



<!-- Rending component conditionally -->
    <!-- v-if & v-else, creates and recreates the object based on conditional -->
			<form class="m-5" v-if="!submited">
                ...
            </form>
            <h2 v-else  class="m-5">Thanks for submiting the form! </h2>
    <!-- v-show , works similiar to v-if but doesn't destroy and create the element
        instead it simply changes the css to hidden or show  -->
            <form class="m-5" v-show="!submited">
                ...
            </form>
            <h2 v-show="submited"  class="m-5">Thanks for submiting the form! </h2>
    <!-- Sometimes you will notice a flicker as vue renders the object
         to avoid this use v-cloak & set a style tag for the attribute [v-cloak] -->
            <style>
                [v-cloak] {
                    display:none;
                }
            </style>

            <h2 v-show="submited" v-cloak  class="m-5">Thanks for submiting the form! </h2>

<!-- Button to submit method -->
	<td><button @click="remove(index)" class="btn btn-warning">Remove </button></td>

<!-- Loading data from API using axios, Recieves a json from nasa and retrives two required values to display image -->


	<img :src="imgSrc" :title="imgTitle">


    data() {
		return {
			email: 'curtis@example.com',
			submited: false,
			apiKey: 'ba4jJKeqIdieZ53aT4qtfjrRbXtgO9TJFBh09eVP',
			imgSrc: '',
			imgTitle: ''

		}
	},
	created: function() { <!-- Runs on creation of page -->
		this.fetchAPOD();
	},
	methods:{
		process: function(){
			this.submited = true;
		},
		fetchAPOD: function(){
			const axios = require('axios').default;
			var url = 'https://api.nasa.gov/planetary/apod?api_key='+ this.apiKey
			axios
			.get(url)
			.then(response => (this.imgSrc = response.data.url, this.imgTitle = response.data.title ))
		}

	}

<!-- Using v-for directive to render arrays  -->

	<table class="table">
		<thead class="thead-dark"> 
			<th>  Name </th>
			<th> Close approach Date </th>
		</thead>
		<tbody v-cloak> <!-- Data may require loading so cloak to avoid flicker -->
			<tr v-for="a in asteroids" :key="a" > 
			<!-- Requires a key to work on and an array  -->
				<td>{{a.name}}</td> 
				<td>{{getCloseApproachDate(a)}}</td>
			</tr>
		</tbody>
	</table>




	data() {
			return {
				asteroids: []

			}
		},
		methods:{
			<!-- Ensures a value is present or returns N/A  -->
		getCloseApproachDate: function(a) {
					if( a.close_approach_data.length > 0){
						return a.close_approach_data[0].close_approach_data;
					}
					return 'N/A'
				}
		}

	<!-- Advanced v-for syntax -->
		<!-- This allows you to also print the element index -->
		<tr v-for="(a, index) in asteroids" :key="a" > 

		<!-- print out object details within json -->
			<ul  v-if="a.close_approach_data.length">
				<li v-for="(value, key) in a.close_approach_data[0].miss_distance" :key="value"> 
					{{key}}: {{value}}
				</li>
			</ul>

<!-- Computed properties, look like methods but are used like data properties 
	 Items that cant be defined as they relly on other data are best done as computed
	 it keeps a cached value and will update it any time a refrenced variable is updated  -->
	<div class="m-3" v-cloak v-if="numAsteroids">
		<p>showing {{numAsteroids}} items </p>
		<p> {{closestObject}}  that has shortest miss distance </p>
	</div>

	computed: {
		numAsteroids: function() {
			return this.asteroids.length
		},
		closestObject: function() {
			<!-- returns all items based on a filter(length > 0) others are discarded -->
			var neosHavingDate = this.asteroids.filter(function (neo){ 
				return neo.close_approach_data.length > 0;
			})
			<!-- returns a simpler array of objects that only contains a variable for name & miles -->
			var simpleNeos = neosHavingDate.map(function (neo){
				return {name: neo.name, miles: neo.close_approach_data[0].miss_distance.miles}
			})
			<!-- Compares the two items sent in and returns the larger -->
			var sortedNeos = simpleNeos.sort(function (a,b) {
				return a.miles - b.miles;
			})
			return sortedNeos[0].name <!-- returns the name of closest asteroid -->
		}
	},

<!-- Conditionally update CSS, binded css will overide regular style tags  -->

		<tr v-for="(a, index) in asteroids" :key="a"
			:style="getRowStyle(a)" style="color: green" >

		methods:{
			getRowStyle: function(a) {
				if (a.close_approach_data.length == 0){
					return { border: 'solid 3px red', color: 'red'}
				}
			}
	
		<!-- Using binded class attributes -->

			<tr v-for="(a, index) in asteroids" :key="a"
				:class="{highlight:true}" >


			<style>
				.highlight {
					border:solid 3px red;
					color: red;
				}
			</style>
		
		<!-- Using Dynamic class binded Attributes -->
			<tr v-for="(a, index) in asteroids" :key="a"
				:class="{highlight: isMissingData(a)}" >

			methods:{
				isMissingData: function(a) {
					return a.close_approach_data.length == 0;
				}
			}

			<style>
				.highlight {
					border:solid 3px red;
					color: red;
				}
			</style>

<!-- Using CSS and Vue to Perform Transitions -->
	<!-- using vue transition component https://vuejs.org/v2/guide/transitions.html  -->

		<transition name="shooting-star">
			<div class="m-3" v-cloak v-if="numAsteroids && showSummary">
				<p>showing {{numAsteroids}} items </p>
				<p> {{closestObject}}  that has shortest miss distance </p>
			</div>
		</transition>
		<div class="m-3">
			<a href="#" @click="showSummary = !showSummary"> show/hide summary </a>
		</div>

		<style >
			.shooting-star-leave-to, .shooting-star-enter{
			transform: translateX(150px) rotate(30deg);
			opacity: 0;
			}

			.shooting-star-enter-active, .shooting-star-leave-active {
				transition: all .5s ease;
			}
		</style>
	
	<!-- Transition Group Component -->
		<!-- This can cause some issues with older browsers and dom rendering -->
					<transition-group tag="tbody" v-cloak name="neo-list">	 
						<tr v-for="(a, index) in asteroids" :key="a"
							:class="{highlight: isMissingData(a)}" >
							<td> {{index + 1}} </td>
							<td>{{a.name}}</td> 
							<td>{{getCloseApproachDate(a)}}</td>
							<td>
								<ul  v-if="a.close_approach_data.length">
									<li v-for="(value, key) in a.close_approach_data[0].miss_distance" :key="value"> 
										{{key}}: {{value}}
									</li>
								</ul>
							</td>
							<td><button @click="remove(index)" class="btn btn-warning">Remove </button></td>
						</tr>
					</transition-group>

					<style >
						.neo-list-leave-to, .neo-list-enter{
							opacity: 0;
							transform: translateY(30px);
						}

						.neo-list-enter-active, .neo-list-leave-active {
							transition: all 1s linear;
						}					
					</style>

		<!-- Fix for browsers that render the dom wrong, is directs vue to know -->
					<tbody is="transition-group" v-cloak name="neo-list">	 
						<tr v-for="(a, index) in asteroids" :key="a"
							:class="{highlight: isMissingData(a)}" >
							<td> {{index + 1}} </td>
							<td>{{a.name}}</td> 
							<td>{{getCloseApproachDate(a)}}</td>
							<td>
								<ul  v-if="a.close_approach_data.length">
									<li v-for="(value, key) in a.close_approach_data[0].miss_distance" :key="value"> 
										{{key}}: {{value}}
									</li>
								</ul>
							</td>
							<td><button @click="remove(index)" class="btn btn-warning">Remove </button></td>
						</tr>
					</tbody>


			<!-- CSS ANIMATIONS, Abunch of transforms tied togethor  -->

					<transition name="spin" appear> <!-- appear makes it happen instantly -->
						<span style="display:inline-block">
							object 
						</span>
					</transition>

					<style >
						.spin-enter-active {
							animation: spin-steps 2s;
						}
						<!-- % of animation completion { action at that point} -->
						@keyframes spin-steps {
							0% {
								transform: scale(1) rotate(0); 
							}
							50% {
								transform: scale(10) rotate(260deg);
							}
							100% {
								transform: scale(1) rotate(1080deg);
							}

						}
					</style>

<!-- Importing components -->

		<template>
			<div id="app">
				<apod date="2018-08-07"></apod>
			</div>
		</template>

		<script>
		import apod from './components/apod.vue';
		export default {
			name: 'App',
			components: {
				apod
			},
			data() {
				return {
					email: 'curtis@example.com',
					submited: false,
					apiKey: 'ba4jJKeqIdieZ53aT4qtfjrRbXtgO9TJFBh09eVP',
					asteroids: [],
					showSummary: true,

				}
			}
		}
		</script>
		<!-- apod class, contains prop(var its expecting) -->
		<template>
			<div id="app">
				<img :src="imgSrc" :title="imgTitle">
			</div>
		</template>
		<script>
		export default {
			name: 'App',
			components: {
			},
			props:['date'],
			data() {
				return {
					imgSrc: '',
					imgTitle: ''
				}
			},
		
		}
		</script>

<!-- Slot tags, Used to pass in data or display a default -->


	<template>
		<div>
			<apod> 
				<p>Slot info is passed</p> <!-- this gets passed to slot holder -->
			</apod>
			<apod date="2018-08-07"></apod> <!-- uses default slot -->
		</div>
	</template>


	<template>
		<div>
			<img :src="imgSrc" :title="imgTitle">
			<slot><p>Unknown date (default slot)</p></slot> <!-- slot tags, contains default slot -->
		</div>
	</template>