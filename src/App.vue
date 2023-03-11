<template>
	<div>
		<CardContent/>
	</div>
</template>

<script>
import CardContent from '@/components/CardContent.vue'

export default {
	name: 'App',
	components:{
		CardContent
	}
}
</script>

<style>
body{
	background-color: #e3e3e3;
}
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
}

</style>
