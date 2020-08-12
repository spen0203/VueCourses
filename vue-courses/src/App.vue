<template>
	<div id="app">
		<div id="container">
			<!--Rending component conditionally using v-if -->
			<form class="m-5" v-show="!submited">
				<div class="form-group">
					<label for="email">Email</label>
					<!-- One way data bind shorthanded
					<input type="email" :value="email" class="form-control form-control-lg">
					-->

					<!--Two way bind -->
					<input type="email" v-model="email" class="form-control form-control-lg">
				</div>
				<!--@ is shorthand for v-on a directive to a method, .prevent stops refreshing of page on call -->
				<button type="submit" @click.prevent="process" class="btn btn-primary">Submit</button>
			</form>
			<!-- v-else contributes to v-if conditionals -->
			<h2 v-show="submited" v-cloak  class="m-5">Thanks for submiting the form! </h2>
			
			<grid> </grid>

			<apod> 
				<p slot="title"> First image </p>
				<p slot="caption">Slot info is passed</p>
			</apod>
			<apod date="2018-08-07"></apod>
		</div>
	</div>
</template>

<script>
import apod from './components/apod.vue';
import grid from './components/asteroidGrid.vue';

export default {
	name: 'App',
	components: {
		apod,
		grid,
	
	},
	data() {
		return {
			email: 'curtis@example.com',
			submited: false,

		}
	},
	
	methods:{
		process: function(){
			this.submited = true;
		},
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


</style>
