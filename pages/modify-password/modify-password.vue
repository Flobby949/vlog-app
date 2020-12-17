<template>
	<view>
		<view>
			<input type="text" class="uni-input" v-model="newpassword" placeholder="输入新密码"/>
			<input type="text" class="uni-input" v-model="renewmpassword" placeholder="输入确认密码"/>
		</view>
		<view class="py-2 px-3" >
			<button 
				class="bg-main text-white" 
				style="border-radius: 50rpx;border: 0;" 
				type="primary"
				:disabled="disabled"
				:class="disabled ? 'bg-main-disabled' : ''"
				@click="submit"
			>
				设置
			</button>
		</view>
	</view>
</template>

<script>
	import { mapState } from 'vuex';
	export default {
		data() {
			return {
				newpassword: '',
				renewmpassword: ''
			}
		},
		computed: {
			...mapState({
				user: state => state.user
			}),
		    disabled() {
				return this.newpassword == '' || this.renewmpassword == '';
		    }
		  },
		methods: {
			//验证
			check() {
				if (this.newpassword !== this.renewmpassword) {
					uni.showToast({
						title: '两次密码不一致',
						icon: 'none'
					});
					return false;
				}
				return true;
			},
			submit() {
				console.log("用户信息")
				console.log(this.user)
				if (!this.check()) {
					return;
				}
				let data = {
					phone: this.user.phone,
					password: this.newpassword,
					nickname: this.user.nickname,
					avatar: this.user.avatar,
					gender: this.user.gender,
					birthday: this.user.birthday,
					address: this.user.address
				};
				this.$H.post('/user/update', data).then(res => {
					this.$store.commit('login', res);
					uni.navigateBack({
						delta: 4
					})
					uni.showToast({
						title: '修改密码成功',
						icon: 'none'
					});
				});
			}
		}
	};
</script>

<style>

</style>
