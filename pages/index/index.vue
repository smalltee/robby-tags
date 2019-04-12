<template>
	<view>
		<robby-tags :tag-data="tabData" :bg-color-type="colorType" :enable-del="showDel" :enable-add="showAdd"></robby-tags>
	</view>
</template>

<script>
	import robbyTags from '@/components/robby-tags/robby-tags.vue'
	
	export default {
		components:{robbyTags},
		data() {
			return {
				tabData:['物理','数学'],
				showDel: true ,
				showAdd: false,
				colorType: 'error'
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
