<template>
	<view>
		<robby-tags :enable-del="showDel"></robby-tags>
	</view>
</template>

<script>
	import robbyTags from '@/components/Robby-tags/Robby-tags.vue'
	
	export default {
		components:{robbyTags},
		data() {
			return {
				// tabData:['物理','数学'],
				showDel: true,
// 				colorType: 'error'
			}
		},
		methods:{
			clickTag: function(e){
				console.log(e)
			},
			deleteTag: function(e){
				console.log(e)
			}
		}
	}
</script>

<style>
	
</style>
