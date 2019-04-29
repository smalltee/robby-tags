<template>
	<view class="">
		<robby-multi-audio v-model="audioList" :total-page-number="totalPageNumber"></robby-multi-audio>
	</view>
</template>

<script>
	import robbyMultiAudio from '@/components/robby-multi-audio/robby-multi-audio.vue'
	
	export default {
		data(){
			return {
				totalPageNumber: 3,
				audioList:[]
			}
		},
		components:{robbyMultiAudio}
	}
</script>

<style>
</style>
