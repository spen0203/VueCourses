<template>
			<!-- v-for to iterate lists  -->
			<div class="card mt-5">
				<h2 class="card-header">
					Near earth 
					<transition name="spin" appear> <!-- appear makes it happen instantly -->
						<span style="display:inline">
							object 
						</span>
					</transition>

				</h2>

				<transition name="shooting-star">
					<div class="m-3" v-cloak v-if="numAsteroids && showSummary">
						<p>showing {{numAsteroids}} items </p>
						<p> {{closestObject}}  that has shortest miss distance </p>
					</div>
					

				</transition>
					<div class="m-3">
						<a href="#" @click="showSummary = !showSummary"> show/hide summary </a>
					</div>







				<table class="table table-striped">
					<thead class="thead-dark"> 
						<th>index</th>
						<th> Name </th>
						<th> Close approach Date </th>
						<th> Miss dist </th>
						<th> ctrl </th>
					</thead>
						
					<tbody is="transition-group" v-cloak name="neo-list">	 
						<tr v-for="(a, index) in asteroids" :key="a"
							:class="{highlight: isMissingData(a)}" >

							<td> {{index + 1}} </td>
							<!-- Requires a key to work on and an array  -->
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
					
					
					


				</table>
			</div>
</template>

<script>
export default {
	name: 'App',
	components: {
	},
	data() {
		return {
			apiKey: 'ba4jJKeqIdieZ53aT4qtfjrRbXtgO9TJFBh09eVP',
			asteroids: [],
			showSummary: true,

		}
	},
	created: function() {
		this.fetchAsteroids();
	},
	computed: {
		numAsteroids: function() {
			return this.asteroids.length
		},
		closestObject: function() {
			// returns all items based on a filter(length > 0) others are discarded -->
			var neosHavingDate = this.asteroids.filter(function (neo){ 
				return neo.close_approach_data.length > 0;
			})
			// returns a simpler array of objects that only contains a variable for name & miles -->
			var simpleNeos = neosHavingDate.map(function (neo){
				return {name: neo.name, miles: neo.close_approach_data[0].miss_distance.miles}
			})
			// Compares the two items sent in and returns the larger -->
			var sortedNeos = simpleNeos.sort(function (a,b) {
				return a.miles - b.miles;
			})
			return sortedNeos[0].name // returns the name of closest asteroid -->
		}
	},
	methods:{
		process: function(){
			this.submited = true;
		},
		fetchAsteroids: function(){
			const axios = require('axios').default;
			var url = 'https://api.nasa.gov/neo/rest/v1/feed?start_date=2015-09-07&end_date=2015-09-08&api_key='+ this.apiKey
			axios
			.get(url)
			.then(response => ( this.asteroids = response.data.near_earth_objects['2015-09-07'].slice(0,10) ))
		},
		getCloseApproachDate: function(a) {
			if( a.close_approach_data.length > 0){
				return a.close_approach_data[0].close_approach_date;
			}
			return 'N/A'
		},
		remove: function(index) {
			this.asteroids.splice(index,1);
		},
		getRowStyle: function(a) {
			if (a.close_approach_data.length == 0){
				return { }
			}
		},
		isMissingData: function(a) {
			return a.close_approach_data.length == 0;
		}
		

	}
}
</script>
<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	/*margin-top: 60px;*/
}

[v-cloak] {
    display:none;
}

.highlight {
	border:solid 3px red;
	color: red;
}

.shooting-star-leave-to, .shooting-star-enter{
	transform: translateX(150px) rotate(30deg);
	opacity: 0;
}

.shooting-star-enter-active, .shooting-star-leave-active {
	transition: all .5s ease;
}




.neo-list-leave-to, .neo-list-enter{
	opacity: 0;
	transform: translateY(30px);
}

.neo-list-enter-active, .neo-list-leave-active {
	transition: all 1s linear;
}

.spin-enter-active {
	animation: spin-steps 2s;
}

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
