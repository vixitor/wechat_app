<template>
	<view class="container">
		<view v-if="loading" class="loading">加载中。。。</view>
		<view v-else>
			<view class="header">好友界面</view>
			<view class="header">您有{{this.friends.length}}个好友</view>
			<view v-for="friend in friends" :key="friend.name" class="users_card">
				<view>{{friend.name}}</view>
				<image :src="friend.avatar"></image>
				<view>详细信息：{{friend.detail}}</view>
				<view>联系方式：{{friend.contact}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loading : false,
				friends : [],
				name : ""
			}
		},
		methods: {
			async fetch_data(){
				const db = uniCloud.database()
				let res = await db.collection("users").get()
				console.log(res)
				let self = await db.collection("users").where({
					name:this.name
				}).get()
				self = self.result.data[0]
				self = self.friends
				console.log(self)
				for(let i = 0;i < self.length;i++){
					let data = await db.collection("users").where({
						name:self[i]
					}).get()
					data = data.result.data[0]
					this.friends.push(data)
				}
				console.log(this.friends)
				this.$forceUpdate()
			}
		},
		onShow() {
			let info = uni.getStorageSync("login_info")
			console.log(info.name)
			if(!info.is_login){
				uni.redirectTo({
					url:"/pages/login/login"
				})
			}
			this.name = info.name
			this.fetch_data()
		}
	}
</script>

<style>
	.header{
		font-size:14px ;
		font-weight: bold;
	}
</style>
