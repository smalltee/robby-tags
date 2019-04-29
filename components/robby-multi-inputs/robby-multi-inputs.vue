<template>
	<view class="inputsList">
		<view class="inputsItem" v-for="(item, index) in value" :key="index">
			<picker class="pageNumber" @change="changePage" :value="item.page" :data-itemIndex="index" :range="pageList">
				<view  class="uni-input">{{item.page===0?'选一页':'第'+item.page+'页'}}</view>
			</picker>
			<input type="text" @input="inputSubtitle" v-model="item.text" placeholder="输入该页的字幕" class="uni-input subtitle"/>
			<view class="deleteInput" :data-itemIndex="index" @tap="deleteSubtitle">
				<uni-icon size="20" type="trash"></uni-icon>
			</view>
		</view>
		<view class="newInput" @tap="createSubtitle">+</view>
	</view>
</template>

<script>
	import uniIcon from '../uni-icon/uni-icon.vue'
	export default {
		name:'robby-multi-inputs',
		props:['value', 'totalPageNumber'],
		data() {
			return {

			}
		},
		components:{uniIcon},
		computed: {
			pageList: function(){
				var arr = ['选一页']
				for(var i=1;i<=this.totalPages;i++){
					arr.push('第'+i+'页')
				}
				return arr
			},
			totalPages: function(){
				return this.totalPageNumber || 0
			} 
		},
		methods: {
			changePage: function(e){
				var itemIndex = e.currentTarget.dataset.itemindex
				var selectedPage = e.detail.value
				this.value[itemIndex].page = parseInt(selectedPage)
				this.$emit('input', this.value)
			},
			createSubtitle: function(){
				if(this.value.length>=this.totalPages){
					return
				}
				
				this.value.push({
					page:0,
					text:''
				})
				this.$emit('input', this.value)
			},
			inputSubtitle: function(){
				this.$emit('input', this.value)
			},
			deleteSubtitle: function(e){
				var itemIndex = parseInt(e.currentTarget.dataset.itemindex)
				this.value.splice(itemIndex,1)
				this.$emit('input', this.value)
			}
		}
	}
</script>

<style>
.inputsItem{
	display: flex;
	margin: 20upx;
}

.pageNumber{
	width: 130upx;
}

.subtitle{
	border: 1px solid gray;
	border-radius: 10upx;
	padding: 5upx;
	flex-grow: 1;
	margin-right: 10upx;
}

.deleteInput{
	display: flex;
	align-items: center;
}
.newInput{
	border:1upx solid black;
	width:60upx;
	margin:20upx 40upx;
	border-radius:10upx;
	height:60upx;
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>