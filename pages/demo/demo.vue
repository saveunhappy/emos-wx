<template>
	<view>
		<view>{{username}}</view>
		<view v-for="one in tel">{{one}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username:"Scott",
				tel:['12341231231','123134234234']
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
