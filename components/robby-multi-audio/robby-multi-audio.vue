<template>
	<view class="inputsList">
		<view class="inputsItem" v-for="(item, index) in value" :key="index">
			<picker class="pageNumber" @change="changePage" :value="item.page" :data-itemIndex="index" :range="pageList">
				<view  class="uni-input">{{item.page===0?'选一页':'第'+item.page+'页'}}</view>
			</picker>
			<view class="micInput" @tap="recordHandle" :data-itemIndex="index">
				<uni-icon v-bind:type="isRecording? 'mic-filled':'mic'" size="20"></uni-icon>
			</view>
			<view class="audioInput" :data-voicepath="item.voicePath" @tap="playAudio" v-if="item.voicePath!==''">
				<uni-icon type="sound" v-bind:color="isPlaying?'red':''" size="20"></uni-icon>
			</view>
			<view class="deleteInput" :data-itemIndex="index" @tap="deleteAudio">
				<uni-icon size="20" type="trash"></uni-icon>
			</view>
		</view>
		<view class="newInput" @tap="createAudio">+</view>
	</view>
</template>

<script>
	import uniIcon from '../uni-icon/uni-icon.vue'
	
	const recorderManager = uni.getRecorderManager()
	const audioContext = uni.createInnerAudioContext()
	
	export default {
		name:'robby-multi-audio',
		props:['value', 'totalPageNumber'],
		data() {
			return {
				isRecording: false,
				isPlaying: false,
				recordingIndex: -1
			}
		},
		created: function(){
			console.log('hello')
			let self = this
			recorderManager.onStop(function(res){
				console.log('audio path:' + res.tempFilePath)
				this.$emit('input', this.value)
				self.value[self.recordingIndex].voicePath = res.tempFilePath
				self.recordingIndex = -1
			})
			
			audioContext.onStop(function(res){
				self.isPlaying = false
			})
			
			audioContext.onEnded(function(res){
				self.isPlaying = false
			})
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
			createAudio: function(){
				if(this.value.length>=this.totalPages){
					return
				}
				
				this.value.push({
					page:0,
					voicePath:''
				})
				this.$emit('input', this.value)
			},
			deleteAudio: function(e){
				var itemIndex = parseInt(e.currentTarget.dataset.itemindex)
				this.value.splice(itemIndex,1)
				this.$emit('input', this.value)
			},
			recordHandle: function(e){
				this.isRecording = !this.isRecording
				if(this.isRecording){//开始录音
					//检查当期是否在播放音频
					if(this.isPlaying){
						this.playAudio()
					}
					
					//记录当前录音的页数
					this.recordingIndex = parseInt(e.currentTarget.dataset.itemindex)
					recorderManager.start()
				}else{//停止录音
					recorderManager.stop()
				}
			},
			playAudio: function(e){
				//检查当前是否在录音，录音时不能播放
				if(this.isRecording){
					uni.showToast({
						title:'当前正在录音，不能播放',
						icon:'none'
					})
					return
				}
				
				this.isPlaying = !this.isPlaying
				if(this.isPlaying){
					var voicepath = e.currentTarget.dataset.voicepath
					audioContext.src = voicepath
					audioContext.play()
				}else{
					audioContext.stop()
				}
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

.micInput{
	padding:0 30upx;
}

.audioInput{
	border: 1px solid gray;
	border-radius: 30upx;
	padding: 5upx;
	flex-grow: 1;
	margin-right: 10upx;
	text-align: center;
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