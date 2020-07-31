<template>
	<div id="app">
		<h1>Записи сотрудников</h1>
		<hr/>
		<passport/>
	</div>
</template>

<script>
	import passport from "./components/passport";
		export function localGet(key){
			JSON.parse(localStorage.getItem(key))
		return JSON.parse(localStorage.getItem(key))
	}
	export function localSet(key,val){
		return localStorage.setItem(key, JSON.stringify(val))
	}
	export function localRemove(key){
			return localStorage.removeItem(key)
	}
	let emp_mas
	if (localGet("staff") == null)
	{
		emp_mas = [
			["Иванов Иван Иванович", "4142", "312414", "2000-03-21"],
			["Лаврентьев Дмитрий Иванович", "3124", "123412", "2000-04-22"]
		]
		localSet("staff",emp_mas)
	}
	export default {
		name: "App",
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
