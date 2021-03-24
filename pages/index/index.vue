<template>
	<view class="uni-padding-wrap">
		<form @submit="commitComments">
			<textarea class="contentTextArea" id="content" name="content" v-model="content" placeholder="请输入您的留言......"></textarea>
			<button class="commitButton" formType="submit">留言</button>
		</form>
		<view class="uni-comment">
			<view class="uni-comment-list" v-for="(commentsList,index) in commentsList" :key="index">
				<view class="uni-comment-body">
					<!-- <view class="uni-comment-date">
						<text>{{commentsList.time}}</text>
					</view> -->
					<view class="uni-comment-content">{{commentsList}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				commentsList: [],
				index: '',
				content: '',
			}
		},
		onLoad() {
			uni.request({
				url: 'http://192.168.0.29:12080/liuyan/display',
				method: 'POST',
				data: {},
				success: res => {
					console.log(res+"----------------");
					this.commentsList = res.data.data;
				}
			});
		},
		methods: {
			commitComments() {
				//这里定义你自己要请求添加留言接口带的值，我是从接口获取到了
				var content = this.content;
				var commentsList = this.commentsList;
				uni.request({
					url: 'http://localhost:12080/liuyan/save',
					method: 'POST',
					header: {
						'content-type': "application/x-www-form-urlencoded"
					},
					//因为我要传递数组格式的comment:{content:'',userid:''}给后台所以这样写，你直接传字段的话就去掉前面的comment和中括号就好了。
					data: {
						'content': content,
					},
					success: function(res) {
						if (res.data.success == 1) {
							//这是重点，意思是动态添加，使用unshift就插在前面，也可以用push							
							commentsList.total += 1;
							commentsList.push({
								"content": content,
							});
						} else {
							uni.showToast({
								title: res.data.data,
								icon: "none"
							});
						}
					}
				})
			},
		}
	}
</script>

<style>
	.uni-padding-wrap {
		padding: 30upx;
	}

	view {
		font-size: 28upx;
	}

	.uni-comment {
		padding: 5rpx 0;
		display: flex;
		flex-grow: 1;
		flex-direction: column;
		margin-top: 20rpx;
		border-bottom: 1rpx solid #e9e7ef;
	}

	.uni-comment-list {
		flex-wrap: nowrap;
		padding: 10rpx 0;
		margin: 10rpx 0;
		width: 100%;
		display: flex;
		border-bottom: 1rpx solid #e9e7ef;
	}

	.uni-comment-body {
		width: 100%;
	}

	.uni-comment-date {
		line-height: 38upx;
		flex-direction: row;
		justify-content: space-between;
		display: flex !important;
		flex-grow: 1;
		color: gray;
	}

	.uni-comment-date view {
		color: #666666;
		font-size: 24upx;
		line-height: 38upx;
	}

	.uni-comment-content {
		line-height: 1.6em;
		font-size: 28upx;
		padding: 8rpx 0;
	}

	.uni-comment-replay-btn {
		background: #FFF;
		font-size: 24upx;
		line-height: 28upx;
		padding: 5rpx 20upx;
		border-radius: 30upx;
		color: #333 !important;
		margin: 0 10upx;
	}

	.commitButton {

		color: white;
		width: 90%;
		margin: 0rpx auto;
		height: 75rpx;
		line-height: 75rpx;
		font-size: 37rpx;
		background: linear-gradient(left top, #86B6FD, #8fc9fc, #86B6FD);
	}

	.contentTextArea {
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
