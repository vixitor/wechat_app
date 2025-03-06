<template>
	<view class="container">
		<view v-if="loading" class="loading">加载中。。。</view>
		<view v-else>
			<view v-for="user in users" :key="user._id" class="users_card">
				<view>
					<image :src="user.avatar" mode=""></image>
				</view>
				<view>姓名:{{user.name}}</view>
				<view>年龄:{{user.age}}</view>
				<view>MBTI:{{user.mbti}}</view>
				<view>爱好:{{users.hobbies}}</view>
				<view>个人介绍:{{user.self_introduction}}</view>
				<view>
				  <image :src="user.image" :style="{ width: '300px', height: '200px' }"></image>
				</view>
				<button @click="like(user.name)">喜欢</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				users:[],
				loading:true,
				name:"",
				mbti:"",
				hobbies:[],
				current_likes:[],
				friends:[]
			}
		},
		onShow(){
			this.init_data()
			this.fetch_users();
		},
		methods: {
			async init_data(){
				this.name = uni.getStorageSync("login_info").name
				console.log(`name : ${this.name}`)
				const db = uniCloud.database()
				let res = await db.collection("users").where({
					name:this.name
				}).get()
				res = res.result.data[0];
				console.log(res)
				this.mbti = res.mbti
				this.hobbies = res.hobbies
				this.current_likes = res.likes
				this.friends = res.friends
				console.log(this.current_likes)
			},
			async fetch_users(){
				this.loading=true
				console.log(this.name)
				try{
					const db = uniCloud.database()
					const res = await db.collection("users").get();
					if(res.result.data){
						this.users = res.result.data;
					}
				}catch(error){
					console.error("数据获取失败",error);
					uni.showToast({
						title:"数据获取失败",
						icon:'none',
						duration:2000
					});
				}finally{
					this.loading=false;
				}
			},
			async like(obj){
				if(this.name == ''){
					uni.redirectTo({
						url:"/pages/login/login"
					})
				}
				else {
					console.log(obj)
					const db = uniCloud.database()
					this.current_likes.push(obj)
					db.collection("users").where({
						name:this.name
					}).update({
						likes:this.current_likes
					})
					let res = await db.collection("users").where({
						name:this.name
					}).get()
					res = res.result.data[0]
					console.log(res.likes)
					let oth = await db.collection("users").where({
						name:obj
					}).get()
					oth = oth.result.data[0]
					console.log(oth.likes)
					console.log(this.name)
					let flag = 0
					for(let i = 0;i < oth.likes.length;i++){
						if(oth.likes[i] == this.name){
							flag = 1
						}
					}
					if(flag == 1){
						oth.friends.push(this.name)
						db.collection("users").where({
							name:obj
						}).update({
							friends:oth.friends
						})
						this.friends.push(obj)
						db.collection("users").where({
							name:this.name
						}).update({
							friends:this.friends
						})
					}
				}
			}
		}
		
	}
</script>

<style>
/* 基础布局样式 */
.container {
  padding: 20px;
  background-color: #f5f5f5;
}

.loading {
  text-align: center;
  font-size: 18px;
  color: #007BFF;
  padding: 20px;
}

.users_card {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.users_card > view {
  margin-bottom: 10px;
}

.users_card image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 50%;
  margin-bottom: 15px;
}

.users_card .user-info {
  font-size: 14px;
  color: #333;
  text-align: center;
  margin-bottom: 10px;
}

.users_card .user-info > view {
  margin-bottom: 5px;
}

.users_card .user-info .name {
  font-size: 16px;
  font-weight: bold;
}

.users_card .user-info .hobbies {
  color: #007BFF;
  font-size: 14px;
}

.users_card .user-info .self_introduction {
  color: #666;
  font-size: 12px;
  line-height: 1.5;
}

/* 为图片添加样式，使用 .tupian 或 #tupian 根据实际情况 */
.tupian {
  width: 100%;
  max-width: 500px;  /* 增大图片的最大宽度 */
  height: auto;
  object-fit: cover;  /* 保持图片比例，裁剪不变形 */
  display: block;
  margin: 20px auto;  /* 居中显示，且上/下有间距 */
  border-radius: 8px;  /* 给图片添加圆角 */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);  /* 给图片添加阴影 */
}

/* 响应式设计：确保在小屏幕上图片自适应 */
@media (max-width: 600px) {
  .tupian {
    max-width: 100%;  /* 在小屏幕上，图片最大宽度为屏幕宽度 */
  }
}

button {
  background-color: #007BFF;
  color: white;
  padding: 8px 16px;
  border-radius: 4px;
  border: none;
  font-size: 14px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #0056b3;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* 响应式样式（调整手机端显示） */
@media (max-width: 600px) {
  .users_card {
    padding: 10px;
  }

  .users_card user.avatar {
    width: 80px;
    height: 80px;
  }
  
  .users_card tupian {
    width: 80%;
    height: auto;
  }

  .users_card .user-info {
    font-size: 12px;
  }

  .users_card .user-info .name {
    font-size: 14px;
  }

  button {
	  background-color: #fd2054;
    padding: 6px 12px;
  }
}
</style>

