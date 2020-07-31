<template>
	<div id="app">
		<h1>Записи сотрудников</h1>
		<hr/>
		<passport v-bind:sotr="sotr"/>
	</div>
</template>

<script>
	import passport from "./components/passport";
	let methods = {
		localGet: function(key){
		return JSON.parse(localStorage.getItem(key))
	},
	localSet: function(key,val){
		return localStorage.setItem(key, JSON.stringify(val))
	}
	};
	let sotr
	if (methods.localGet("Massotr") != null)
		sotr = methods.localGet("Massotr")
	else {
		sotr = [
			["Иванов Иван Иванович", "4142", "312414", "2000-03-21"],
			["Лаврентьев Дмитрий Иванович", "3124", "123412", "2000-04-22"]
		]
		methods.localSet("Massotr",sotr)
	}
	export default {
		name: "App",
		data() {
			return {
				sotr,
				methods
			}
		},
		components: {
			passport,
		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
