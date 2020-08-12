<template>
    <div>
        <slot name="title">
            <p>Unknown Title (default slot)</p>
        </slot>
        <img :src="imgSrc" :title="imgTitle">
        <slot name="caption">
            <p>Unknown date (default slot)</p>
        </slot>
    </div>
</template>
<script>

export default {
	name: 'Apod',
	components: {
    },
    props:['date'],
	data() {
		return {
            apiKey: 'ba4jJKeqIdieZ53aT4qtfjrRbXtgO9TJFBh09eVP',
			imgSrc: '',
			imgTitle: ''
		}
    },
    created: function() { 
		this.fetchAPOD();
	},
	methods:{
		process: function(){
			this.submited = true;
		},
		fetchAPOD: function(){
			const axios = require('axios').default;
			var url = 'https://api.nasa.gov/planetary/apod?api_key='+ this.apiKey
            if(this.date) {
                url += '&date=' + this.date;
            }
            
            axios
			.get(url)
			.then(response => (this.imgSrc = response.data.url, this.imgTitle = response.data.title ))
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


</style>
