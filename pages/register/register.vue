<template>
	<view>
		<image src="../../static/logo-2.png" mode="widthFix" class="logo"></image>
		<view class="register-container">
			<input type="text" placeholder="输入你的邀请码" class="register-code" maxlength="6" v-model="registerCode" />
			<view class="register-desc">管理员创建员工证账号之后，你可以从你的个人邮箱中获得注册邀请码</view>
			<button class="register-btn" @tap="register()">执行注册</button>
		</view>
	</view>
</template>
<!-- open-type="getUserInfo" -->
<script>
export default {
	data() {
		return {
			registerCode: ''
		};
	},
	methods: {
		register: function() {
			let _this = this;
			if (_this.registerCode == null || _this.registerCode.length == 0) {
				uni.showToast({
					icon: 'none',
					title: '邀请码不能为空'
				});
				return;
			} else if (/^[0-9]{6}$/.test(_this.registerCode) == false) {
				uni.showToast({
					icon: 'none',
					title: '邀请码必须是6位数字'
				});
				return;
			}
			// uni.login({
			// 	provider: 'weixin',
			// 	success: function(resp) {
			// 		let code = resp.code;
			// 		_this.code = code;
			// 		console.log('code', code);
			// 		// console.log(resp);
			// 	}
			// });
			uni.getUserProfile({
				desc: 'Wexin', // 这个参数是必须的
				success: function(resp) {
					uni.login({
						provider: 'weixin',
						success: function(res) {
							let code = res.code;
							// _this.code = code;
							console.log('code', code);
							// console.log(resp);
							// console.log(resp);
							let nickName = resp.userInfo.nickName;
							let avatarUrl = resp.userInfo.avatarUrl;
							// console.log(nickName);
							// console.log(avatarUrl); 
							let data = {
								code:code,
								nickname:nickName,
								photo:avatarUrl,
								registerCode:_this.registerCode
							}
							
							console.log("register",_this.url.register)
							_this.ajax(_this.url.register,"POST",data,function(resp){
								let permission = resp.data.permission;
								uni.setStorageSync("permission",permission);
								console.log("permission",permission);
								//TODO 跳转到index页面
								uni.switchTab({
									url:"../index/index"
								})
							})
						}
					});

				}
			});
		}
	}
};
</script>

<style lang="less">
@import url('register.less');
</style>
