<template>
	<view>
	<view class=" flex align-stretch mt-2">
		<view class="border-bottom flex align-center justify-center font px-2">+86</view>
		<input type="text" v-model="phone" placeholder="请输入待绑定手机号" class="border-bottom p-2 flex-1" />
	</view>
	<view class="flex align-stretch">
		<input type="text" v-model="code" placeholder="请输入验证码" class="border-bottom p-2 flex-1" />
		<view
			class="border-bottom flex align-center justify-center font-sm text-white"
			:class="codeTime > 0 ? 'bg-main-disabled' : 'bg-main'"
			style="width: 180rpx;"
			@click="getCode"
		>
			{{ codeTime > 0 ? codeTime + ' s' : '获取验证码' }}
		</view>
	</view>
	<view class="mt-4 py-2 px-3" >
		<button 
			class="bg-main text-white" 
			style="border-radius: 50rpx;border: 0;" 
			type="primary"
			:disabled="disabled"
			:class="disabled ? 'bg-main-disabled' : ''"
			@click="submit"
		>
			确定
		</button>
	</view>
	</view>
</template>

<script>
	import { mapState } from 'vuex';
	export default {
		data() {
			return {
				status: false,
				phone: '',
				code: '',
				codeTime: 0,
				newpassword: '',
				renewmpassword: ''
			};
		},
		onLoad() {},
		computed: {
			...mapState({
				user: state => state.user
			}),
			disabled() {
				return this.phone === '' || this.code === '';
			}
		},
		methods: {
			//表单验证
			validate() {
				//手机号正则
				var mPattern = /^1[34578]\d{9}$/;
				if (!mPattern.test(this.phone)) {
					uni.showToast({
						title: '手机格式不正确',
						icon: 'none'
					});
					return false;
				}
				// ...更多验证
				return true;
			},
			//获取验证码
			getCode() {
				console.log("phone"+this.phone);
				this.$H
					.post('/user/verifyPhone?phone=' + this.phone)
					.then(flag => {
						console.log(flag);
						if(flag){
							//防止重复获取
						if (this.codeTime > 0) {
							return;
						}
						//验证手机号
						if (!this.validate()) return;
						//请求数据
						this.$H
							.post('/user/sendcode?phone=' + this.phone, {
								native: true
							})
							.then(res => {
								//倒计时
								this.codeTime = 60;
								let timer = setInterval(() => {
									if (this.codeTime >= 1) {
										this.codeTime--;
									} else {
										this.codeTime = 0;
										clearInterval(timer);
									}
								}, 1000);
							});
						} else {
							uni.showModal({
								title: '手机号已存在！'
							});
						}
					});
			},
			submit() {
				//表单验证
				let url = '';
				let data = '';
					//手机验证码登录
					if (!this.validate()) return;
					url = '/user/bindPhone';
					data = {
						wxOpenId: this.user.wxOpenId,
						phone: this.phone,
						code: this.code
					};
				//提交后端
				this.$H
					.post(url, data)
					.then(res => {
						if (res) {
							console.log(res);
							//持久化存储
							this.$store.commit('login', res);
							//提示和跳转
							uni.showModal({
								title: '绑定成功,去改密码',
								icon: 'none',
								success: function(res) {
									if (res.confirm) {
										uni.navigateTo({
											url: '../modify-password/modify-password'
										});
									} else if (res.cancel) {
										console.log('用户绑定取消');
										return;
									}
								}
							});
						} else {
							uni.showModal({
								title: '绑定失败。验证码错误或该用户已存在。'
							});
							return;
						}
					})
			},
		}
	}
</script>

<style>

</style>
