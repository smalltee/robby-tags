<template>
	<view class="content">
		<robby-multi-inputs v-model="inputsList" :total-page-number="totalPages"></robby-multi-inputs>
	</view>
</template>

<script>
	import robbyMultiInputs from '@/components/robby-multi-inputs/robby-multi-inputs.vue'
	
	export default {
		data() {
			return {
				inputsList:[{
					page: 3,
					text: '话费报吗？'
				}],
				totalPages: 5
			}
		},
		components:{robbyMultiInputs},
		computed: {
			
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
