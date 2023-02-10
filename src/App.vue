<template>
	<div class="app">
		<user-form />
	</div>
</template>

<script>
import UserForm from './components/UserForm.vue'

export default {
	components: { UserForm },
	name: 'App',
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}

.app {
	position: relative;
	background-color: #F7F7F7;
	height: 100vh;
	background-size: 100%;
}
</style>
