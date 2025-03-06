<template>
	<view>修改界面</view>
		<view class="home_header">{{this.name}}，你好</view>
		<view id = "header">个人信息</view>
		<view class="item">
			<label for="age">年龄:</label>
			<input id="age" v-model="age" type="number" :placeholder="`${this.age}`"/>
		</view>
		<view class="item">
			<lable>MBTI：</lable>
		    <input id = "mbti" v-model="mbti" :placeholder="`${this.mbti}`"/>
		</view>
		<view class="item">
		    <label>爱好：</label>
		    <checkbox-group @change="checkbox_change">
				<checkbox value="阅读" /> 阅读
				<checkbox value="运动" /> 运动
				<checkbox value="音乐" /> 音乐
				<checkbox value="旅游" /> 旅游
		    </checkbox-group>
		</view>
		<view class="form-item">
		    <label for="bio">个人简介：</label>
		    <textarea id="bio" v-model="self_introduction" :placeholder="`${self_introduction}`" maxlength="50"/>
		</view>
		<view class="form-item">
		    <label for="detail">全部信息：</label>
		    <textarea id="detail" v-model="detail" :placeholder="`相互喜欢后可见 ${detail}`" maxlength="50"/>
		</view>
		<view id = "contact">联系方式</view>
		<input v-model = "contact" :placeholder="`相互喜欢后可见 ${contact}`"/>
		<!-- <view>{{hobbies}}</view>
		<view>{{age}}</view>
		<view>{{gender}}</view> -->
		<button @click="submit">提交修改</button>
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
				contact:""
			}
		},
		methods: {
			async init_data(){
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
				contact = res.contact
				this.$forceUpdate()
			},
			radio_change(evt){
				console.log("radio_change")
				this.gender = evt.detail.value
			},
			checkbox_change(evt){
				console.log("checkbox_change")
				console.log(evt.detail.value)
				for(let i = 0;i < evt.detail.value.length;i++){
					this.hobbies.push(evt.detail.value[i])
				}
			},
			async submit(){
				console.log("submit already")
				console.log(this.hobbies)
				// console.log({
				// 	age:this.age,
				// 	gender:this.gender,
				// 	hobbies:this.hobbies,
				// 	self_introduction:this.self_introduction
				// })
				// uni.showToast({
				// 	title:`name=${this.name},age=${this.age},gender=${this.gender},hobbies=${this.hobbies}`,
				// 	icon:'none'
				// })
				this.mbti = this.mbti.toUpperCase()
				const db = uniCloud.database()
				db.collection("users").where({
					name:this.name
				}).update({
					age:this.age,
					gender:this.gender,
					mbti:this.mbti,
					self_introduction:this.self_introduction,
					detail:this.detail,
					hobbies:this.hobbies,
					contact:this.contact
				});
				uni.showToast({
					title:"修改成功",
					icon:"success",
					duration:2000
				})
				setTimeout(()=>{
					uni.switchTab({
						url:"/pages/show_profile/show_profile"
					})
				},2000);
			}
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
  /* 整体页面的通用样式 */
  page {
    background-color: #f5f5f5;
    font-family: Arial, sans-serif;
  }
  
  /*问候语样式*/
  .home_header{
	  
  }
  
  /* 个人中心标题样式 */
  view:first-child {
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 10px;
    color: #333;
    text-align: center;
  }
  
  /* 个人信息标题样式 */
  #header {
    font-size: 16px;
    font-weight: bold;
    margin-top: 15px;
    border-bottom: 1px solid #ccc;
    padding-bottom: 5px;
    color: #555;
  }
  
  /* 表单每一项的整体样式 */
  .item,
  .form-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    background-color: white;
    padding: 8px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  /* 表单中label的样式 */
  label {
    min-width: 60px;
    color: #666;
  }
  
  /* 输入框样式 */
  input {
    border: 1px solid #ccc;
    padding: 5px;
    border-radius: 3px;
    flex: 1;
    outline: none;
    transition: border-color 0.3s ease;
  }
  
  input:focus {
    border-color: #007aff;
  }
  
  /* 单选框组、复选框组整体样式 */
  radio-group,
  checkbox-group {
    display: flex;
    flex-wrap: wrap;
  }
  
  /* 单选框、复选框样式 */
  radio,
  checkbox {
    margin-right: 10px;
    margin-bottom: 5px;
  }
  
  /* 选择器样式 */
  .picker {
    border: 1px solid #ccc;
    padding: 5px;
    border-radius: 3px;
  }
  
  /* 文本域样式 */
  textarea {
    border: 1px solid #ccc;
    padding: 5px;
    border-radius: 3px;
    resize: none;
    flex: 1;
    min-height: 80px;
    outline: none;
    transition: border-color 0.3s ease;
  }
  
  textarea:focus {
    border-color: #007aff;
  }
  
  /* 提交按钮样式 */
  button {
    background-color: #5cb9ff; /* 按钮背景设为绿色，与之前按钮风格一致 */
    color: white; /* 按钮文字设为白色 */
    border: none; /* 去掉按钮默认边框 */
    border-radius: 5px; /* 按钮圆角 */
    padding: 5px 15px; /* 适当的内边距，让按钮看起来更饱满 */
    font-size: 16px;
    cursor: pointer; /* 鼠标悬停变为手型，提示可点击 */
    margin-top: 20px; /* 与输入框之间有一定间距 */
    width: 85%; /* 按钮宽度与输入框保持一致，水平居中显示 */
    margin: 30px auto 0; 
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  /* 联系方式标题样式，可与个人信息标题样式保持一致或适当差异化 */
  #contact {
    font-size: 16px;
    font-weight: bold;
    margin-top: 15px;
    border-bottom: 1px solid #ccc;
    padding-bottom: 5px;
    color: #555;
  }
  
  /* 隐藏暂时不用的元素 */
  /* 以下部分根据实际情况决定是否保留隐藏，若后续需要展示对应信息可取消隐藏样式 */
  view[hidden] {
    display: none;
  }
</style>