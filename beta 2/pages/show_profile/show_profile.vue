<template>
  <view class="container">
    <view class="title">个人中心</view>
    
    <view class="header">{{ name }}, 您好</view>
    
    <view class="navigate">
      <navigator url="/pages/profile/profile" open-type="redirect">点击修改</navigator>
    </view>
    
    <view class="avatar-container">
      <image :src="avatar" class="avatar"></image>
      <button @click="upload_avatar">上传头像</button>
    </view>
    <!-- 个人信息板块 -->
    <view class="form-item">
      <view class="form-title">个人信息</view>
      <view class="info-box">
          <view class="info-row">用户名：{{ name }}</view>
          <view class="info-row">年龄：{{ age }}</view>
          <view class="info-row">性别：{{ gender }}</view>
          <view class="info-row">MBTI：{{ mbti }}</view>
          <view class="info-row">爱好：{{ hobbies }}</view>
		</view>
    </view>
    
    <!-- 自我介绍板块 -->
    <view class="form-item">
      <view class="form-title">自我介绍</view>
      <view class="info-box">
        <text>{{ self_introduction }}</text>
      </view>
    </view>
    
    <!-- 详细信息板块 -->
    <view class="form-item">
      <view class="form-title">详细信息</view>
      <view class="info-box">
        <text>{{ detail }}</text>
      </view>
    </view>
    
    <!-- 联系方式板块 -->
    <view class="form-item">
      <view class="form-title">联系方式</view>
      <view class="info-box">
        <text>{{ contact }}</text>
      </view>
    </view>
    <view class="image-container">
      <image :src="image"></image>
      </view>
      <button @click="upload_image">上传图片</button>
    <!-- 可选的提交按钮 -->
    <!-- <button @click="submit">提交修改</button> -->
  </view>
</template>



<script>
	export default {
		data() {
			return {
				name:"",
				age:"",
				gender:"",
				mbti:"",
				hobbies:[],
				self_introduction:"null",
				detail:"null",
				is_login:false,
				contact:"",
				avatar:"",
				image:""
			}
		},
		methods: {
			upload_avatar(){
				uni.chooseImage({
					count:1,
					success:(res)=>{
						uniCloud.uploadFile({
							cloudPath: Date.now() + '.png',
							filePath:res.tempFilePaths[0],
							success: (res) => {
								this.avatar=res.fileID
							}
						})
					},
					fail(err){
						console.log(err)
					}
				})
				const db = uniCloud.database()
				db.collection("users").where({
					name:this.name,
				}).update({
					avatar:this.avatar
				})
				console.log(this.name)
				this.$forceUpdate()
			},
			upload_image(){
				uni.chooseImage({
					count:1,
					success:(res)=>{
						uniCloud.uploadFile({
							cloudPath: Date.now() + '.png',
							filePath:res.tempFilePaths[0],
							success: (res) => {
								this.image=res.fileID
							}
						})
					},
					fail(err){
						console.log(err)
					}
				})
				const db = uniCloud.database()
				db.collection("users").where({
					name:this.name,
				}).update({
					image:this.image
				})
				console.log(this.name)
				this.$forceUpdate()
			},
			async init_data(){
				console.log("init_data")
				const db = uniCloud.database()
				let res = await db.collection("users").where({
					name : this.name
				}).get();
				if(res.result.data.size < 1){
					console.error("no data")
					return;
				}
				res = res.result.data[0];
				this.name = res.name
				this.age = res.age
				this.gender = res.gender
				this.mbti = res.mbti
				this.self_introduction = res.self_introduction
				this.detail = res.detail
				this.contact = res.contact
				this.avatar = res.avatar
				this.image = res.image
				this.hobbies = res.hobbies
				console.log(this.hobbies)
				this.$forceUpdate()
			},
			// radio_change(evt){
			// 	console.log("radio_change")
			// 	this.gender = evt.detail.value
			// },
			// checkbox_change(evt){
			// 	console.log("checkbox_change")
			// 	this.hobbies = []
			// 	for(let i = 0;i < evt.detail.value.length;i++){
			// 		this.hobbies.push(evt.detail.value[i])
			// 	}
			// },
			// async submit(){
			// 	console.log("submit already")
			// 	console.log(this.hobbies)
			// 	// console.log({
			// 	// 	age:this.age,
			// 	// 	gender:this.gender,
			// 	// 	hobbies:this.hobbies,
			// 	// 	self_introduction:this.self_introduction
			// 	// })
			// 	// uni.showToast({
			// 	// 	title:`name=${this.name},age=${this.age},gender=${this.gender},hobbies=${this.hobbies}`,
			// 	// 	icon:'none'
			// 	// })
			// 	this.mbti = this.mbti.toUpperCase()
			// 	const db = uniCloud.database()
			// 	let target = db.collection("users").where({
			// 		name:this.name
			// 	}).update({
			// 		age:this.age,
			// 		gender:this.gender,
			// 		mbti:this.mbti,
			// 		self_introduction:this.self_introduction,
			// 		detail:this.detail
			// 	})
			// 	this.$forceUpdate()
			// }
		},
		onShow(){
			let info = uni.getStorageSync("login_info")
			// console.log(info.is_login)
			// console.log(info.name)
			this.name = info.name
			if(!info.is_login){
				console.log("not found")
					uni.redirectTo({
					url:"/pages/login/login"
				})
			}
			this.init_data();
			console.log(this.name)
			console.log(this.gender)
		},
		// onLoad(){
		// 	this.age = 0
		// 	this.self_introduction = "null"
		// 	this.detail = "null"
		// }
		// onShow(() => {
		// 	let info = uni.getStorage("login_info")
		// 	this.name = info.name
		// 	this.is_login = info.is_login
		// })
		}
</script>

<style>
/* 整体页面布局和样式 */
page {
    background-color: #f5f5f5;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #333;
}

.container {
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 10px;  /* 使页面有圆角 */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);  /* 增加阴影效果 */
}

.title {
  font-size: 28px;
  font-weight: bold;
  color: #007BFF;  /* 标题颜色 */
  margin-bottom: 20px;
  text-align: center;
}

.header {
  font-size: 16px;
  color: #555;  /* 标题下方文字颜色 */
  margin-bottom: 15px;
  text-align: center;
}

.navigate {
  font-size: 18px;
  color: #007BFF;
  text-align: center;
  margin-bottom: 20px;
  cursor: pointer;
}

.avatar-container{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.image-container {
  align-items: center;
  margin-bottom: 20px;
  text-align: center;
}

.avatar{
  width: 80px; /* 更小的头像 */
  height: 80px;
  border-radius: 50%; /* 头像圆形 */
  object-fit: cover;
  margin-right: 20px;
  border: 4px solid #007BFF;  /* 为头像添加边框 */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);  /* 为头像添加阴影 */
}

.image {
  width: 100%;
  max-width: 500px;  /* 增大图片的最大宽度 */
  height: auto;
  object-fit: cover;
  display: block;
  margin: 20px auto;
  border-radius: 8px;  /* 给图片添加圆角 */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);  /* 给图片添加阴影 */
}

button {
  padding: 12px 24px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;  /* 添加过渡效果 */
}

button:hover {
  background-color: #0056b3;  /* 按钮悬停效果 */
}

.form-item {
  background-color: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.form-title {
  font-size: 22px;
  font-weight: bold;
  color: #007BFF;
  margin-bottom: 15px;
}

.info-box {
  background-color: #fafafa;
  padding: 15px;
  border-radius: 8px;
  color: #555;
}

.info-row {
  margin-bottom: 10px;
  font-size: 16px;
}

.info-row span {
  font-weight: bold;
}

.text {
  font-size: 16px;
  color: #333;
}

@media (max-width: 600px) {
  .avatar {
    width: 70px;  /* 在小屏幕上头像更小 */
    height: 70px;
  }

  .image {
    width: 100%;
    max-width: 350px;  /* 小屏幕下图片的最大宽度 */
  }

  button {
    padding: 10px 20px;
    font-size: 14px;
  }

  .form-title {
    font-size: 20px;
  }

  .info-row {
    font-size: 14px;
  }
}
</style>