<template>
	<view>
		<page-head :title="title"></page-head>
		<view class="uni-padding-wrap uni-common-mt">
			<view class="uni-padding-wrap uni-common-mt">
				<form @submit="formSubmit">
					<view class="uni-form-item uni-column">
						<view class="title">请留言</view>
						<input class="uni-input" name="leaveMessage" placeholder="请添加留言" />
					</view>
					<view class="uni-btn-v">
						<button class="addbutton" form-type="submit">"添加留言"</button>
					</view>
				</form>
			</view>
			<view class="text-box" scroll-y="true">
				<text>{{text}}</text>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				title: '留言板',
				text: '',
				extraLine: []
			}
		},
		methods: {
			formSubmit: function(e) {
				var date = new Date(),
					year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second;
				console.log(e);
				console.log(timer);
				this.extraLine.push(timer + JSON.stringify(e.detail.value.leaveMessage));
				this.text = this.extraLine.join('\n');
			}
		}
	}
</script>

<style>
	.text-box {
		font-size: 30rpx;
		height: 500rpx;
		border: 1rpx solid #e9e7ef;
		width: 86%;
		margin: 0rpx auto;
		margin-top: 50rpx;
		margin-bottom: 15rpx;
		padding: 30rpx 0 0 20rpx;
		border-radius: 6rpx;
	}

	.title {
		color: black;
		width: 90%;
		margin: 0rpx auto;
		height: 75rpx;
		line-height: 75rpx;
		font-size: 37rpx;
	}

	.addbutton {
		color: white;
		width: 90%;
		margin: 0rpx auto;
		height: 75rpx;
		line-height: 75rpx;
		font-size: 37rpx;
		background: linear-gradient(left top, #86B6FD, #8fc9fc, #86B6FD);
	}
	.uni-input{
		font-size: 30rpx;
		height: 170rpx;
		border: 1rpx solid #e9e7ef;
		width: 86%;
		margin: 0rpx auto;
		margin-bottom: 15rpx;
		padding: 20rpx 0 0 20rpx;
		border-radius: 6rpx;
	}
</style>
