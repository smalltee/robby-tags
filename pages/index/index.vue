<template>
	<view>
		<view class="uni-label">
			带删除，带添加，primary背景色
		</view> 
		<robby-tags v-model="tabData" :bg-color-type="colorType" :enable-del="showDel" :enable-add="showAdd"></robby-tags>
		<view :key="index" v-for="(tag,index) in tabData">
			{{index}}. {{tag}}
		</view>
	</view>
</template>

<script>
	import robbyTags from '@/components/robby-tags/robby-tags.vue'
	
	export default {
		components:{robbyTags},
		data() {
			return {
				tabData:['建筑','动漫','艺术'],
				showDel: true ,
				showAdd: true,
				colorType: 'primary',
			}
		},
		methods:{
			clickTag: function(e){
				console.log(e)
			},
			deleteTag: function(e){
				console.log(e)
			},
			addTag: function(e){
				console.log(e)
			}
		}
	}
</script>

<style>
	
</style>