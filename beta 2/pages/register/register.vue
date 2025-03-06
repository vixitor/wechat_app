<template>
	<view class="container">
			<image class="image" src="/static/return2.png" mode="widthFix" @click="goToLogin"></image>
	</view>
	 <view id="login_header">注册界面</view>
	<view>
		<input v-model="name" id="name_input" placeholder="输入账号"/>
		<input v-model="password" id="name_input" password type="text" placeholder="输入密码"/>
		<input v-model="password_again" id="name_input" password type = "text" placeholder="请再次输入密码"/>
		<button @click="register" id = "sumbit_button">注册</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name:"",
				password:"",
				password_again:""
			}
		},
		methods: {
			goToLogin(){
					uni.navigateTo({
						url:'/pages/login/login'
					});
			},
			register(){
				if(this.name == ""){
					uni.showToast({
						title:"账号为空",
						icon:'fail',
						duration:2000
					})
				}
				else if(this.password != this.password_again){
					uni.showToast({
						title:"两次输入密码不一致",
						icon:'fail',
						duration:2000
					})
				}
				else {
					const db = uniCloud.database();
					db.collection("users").add({
						name:this.name,
						password:this.password,
						age:0,
						gender:"male",
						mbti:"null",
						hobbies:[],
						self_introduction:"",
						likes:[],
						detail:"",
						friends:[],
						contact:"null"
					})
					uni.showToast({
						title:"注册成功，请登录",
						icon:'success',
						duration:2000
					})
					setTimeout(()=>{
						uni.redirectTo({
							url:"/pages/login/login"
						})
					},2000)
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
	  margin-right: 1;
	}
	
	/* 输入框通用样式 */
	#name_input,
	#password_input {
	  border: 1px solid #ccc;
	  border-radius: 10px;
	  padding: 13px;
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
