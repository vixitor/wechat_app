<template>
	<view class="container">
			<image class="image" src="/static/return1.png" mode="widthFix" @click="goToHome"></image>
	</view>
	<view id="login_header">登录界面</view>
	<input id="name_input" v-model="name" placeholder="请输入账号"/>
	<input id="name_input" v-model="password" placeholder="请输入密码" password type="text"/>
	<button id="sumbit_button" @click="submit" :disabled="is_submitting">登录</button>
	<view id="register_button">
		<navigator url="/pages/register/register" open-type="redirect">
		如果没有账号，请先<span style="color: royalblue">点此注册</span>
		</navigator>
	</view>
	<!-- <view> -->
		<!-- <navigator url = "/pages/profile/profile">主界面</navigator> -->
	<!-- </view> -->
</template>

<script>
	export default {
		data() {
			return {
				name:"",
				password:"",
				is_submitting:false
			}
		},
		methods: {
			goToHome(){
					uni.switchTab({
						url:'/pages/home/home'
					});
			},
			async submit(){
				console.log('正在提交登录请求。。。');
				console.log(this.name)
				console.log(this.password)
				this.is_submitting = true
				const db = uniCloud.database()
				try{
					let res = await db.collection("users").where({
						name:this.name,
						password:this.password
					}).get();
					console.log("result:",res.result.data.length)
					if(res.result.data.length > 0){
						console.log("查询结果 ",res.data)
						uni.showToast({
							title:"登录成功",
							icon:"success",
							duration:2000
						})
						let login_info = {
							name : this.name,
							is_login : true
						}
						uni.setStorageSync("login_info",login_info)
						setTimeout(()=>{
							uni.switchTab({
								url:`/pages/show_profile/show_profile`
							})
						},2000);
					}
					else {
						uni.showToast({
							title:"用户名或者密码错误",
							icon:'none',
							duration:2000
						})
					}
				}catch(err){
					console.error("查询失败 ",err);
					uni.showToast({
						title:this.password,
						icon:'none',
						duration:2000
					});
				}finally{
					this.is_submitting=false;
				}
			}
		}
	}
</script>


<style>
	page {
	  background-color: #f4f4f4;
	}
	
	/* 登录页面头部样式 */
	#login_header {
      margin:20px;
	  font-size: 22px; 
	  font-weight: bold;
	  text-align: center; /* 文字居中显示 */
	  padding: 30px 0; /* 上下添加一定内边距 */
	}
	
	#login_header navigator {
	  text-decoration: none; /* 去掉链接默认下划线 */
	  color: white; /* 链接文字也设为白色 */
	  margin-left: 5px; /* 与前面文字间隔一点距离，更美观 */
	}
	
	/*返回样式*/
	.container {
	  padding: 1px;
	}
	
	.image {
	  width: 35px;
	  margin-right: 20px;
	}
	
	/* 输入框通用样式 */
	#name_input,
	#password_input {
	  border: 1px solid #ccc;
	  border-radius: 10px;
	  padding: 15px;
	  width: 80%;
	  margin: 20px;
	  text-align: center;
	  font-size: 14px;
	  transition: border-color 0.3s ease; /* 添加过渡效果，让边框颜色变化更平滑 */
	}
	
	/* 输入框占位符文字样式 */
	#name_input::placeholder,
	#password_input::placeholder {
	  color: #999; /* 占位符文字设为淡灰色 */
	}
	
	/* 输入框聚焦时的样式 */
	#name_input:focus,
	#password_input:focus {
	  border-color: #2a68db; /* 聚焦时边框变为蓝色，与页面头部提示栏颜色呼应，突出显示 */
	  outline: none; /* 去掉默认的聚焦外边框 */
	}
	
	
	/* 密码输入框整体容器样式 */
	.password-input-container {
	  position: relative;
	  width: 80%;
	  margin: 0 auto;
	}
	/* 密码切换图标容器样式 */
	.password-toggle-wrapper {
	  position: absolute;
	  left:8px;
	  top: 50%;
	  transform: translateY(-50%);
	  width: 20px;
	  height: 20px;
	  cursor: pointer;
	}
	/* 密码切换图标样式 */
	.password-toggle-icon {
	  width: 100%;
	  height: 100%;
	  object-fit: contain; /* 让图片按比例完整显示在图标容器内 */
	}
	
		
	#register_button{
		margin: 10px;
		text-align: center;
		color: #7e7e7e;
	}
	
	/* 登录按钮样式 */
	#sumbit_button {
	  background-color: #5cb9ff; /* 按钮背景设为绿色，与之前按钮风格一致 */
	  color: white; /* 按钮文字设为白色 */
	  border: none; /* 去掉按钮默认边框 */
	  border-radius: 5px; /* 按钮圆角 */
	  padding: 5px 15px; /* 适当的内边距，让按钮看起来更饱满 */
	  font-size: 16px;
	  cursor: pointer; /* 鼠标悬停变为手型，提示可点击 */
	  margin-top: 20px; /* 与输入框之间有一定间距 */
	  width: 85%; /* 按钮宽度与输入框保持一致，水平居中显示 */
	  margin: 30px auto 0; /* 上外边距20px，左右自动居中，上外边距0 */
	}
	
	#sumbit_button:disabled {
	  background-color: #ccc; /* 按钮禁用时变为灰色，提示不可操作 */
	  cursor: not-allowed; /* 鼠标悬停变为禁止图标 */
	}

</style>
