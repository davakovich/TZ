<template>
	<li>
        <span>
            <strong v-on:click="$emit('filler',emp_mas[0])">{{emp_mas[0]}}</strong>
        </span>
	</li>

</template>

<script>
	export default {
		props: {
			emp_mas: {}
		}
	}
</script>
<style>
	.edit {
		margin-right: 10px;
	}

	.remove {
		background: red;
		color: #fff;
		border-radius: 50%;
		font-weight: bold;
	}

	ul {
		list-style: none;
		margin: 0;
		padding: 0;
	}
</style>
