<template>
	<view class="box">
		<view class="hea">
			<view class="Line" :style="{height:height + 'px'}" v-if="show==true"></view>
			<view class="" style="height: 90rpx;">
				<image src="../static/icon/wenjianjia.png" class="image"></image>
				<span class="span1">{{list.name}}</span>
				<!-- <span class="span2" @click="addkaiguan()">展开</span> -->
			</view>
			<view class="hea2" v-for="(item, e) in list.son" v-if="show==true">
				<view style="min-height: 50rpx;">
					<view class="widthss"></view>
					<image src="../static/icon/wenjianjia.png" class="image"></image>
					<span class="span1">{{ item.name }}</span>
					<span class="span2" @click="xianshi(e)" v-if="item.implicit==false">展开</span>
					<span class="span2" @click="xianshi(e)" v-if="item.implicit==true">收起</span>
				</view>
				<view class="hea3" v-for="(add, index) in item.chean" v-if="item.implicit==true&&show==true">
					<image :src="add.img" class="images"></image>
					<span class="span1">{{ add.name }}</span>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: {
				name: '产业带',
				son: [
					{
						name: '研发',
						implicit:false,
						chean: [
							{ name: '连泽贇',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '元呵呵',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '倪亚男',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '是凌霄',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' },
						]
					},
					{
						name: '管理',
						implicit:false,
						chean: [
							{ name: '胡国庆',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '姚永超',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' },
						]
					},
					{
						name: '人事',
						implicit:false,
						chean: [
							{ name: '胡国庆',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '姚永超',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' }, 
							{ name: '姚永超',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' },
						]
					},
					{
						name: '财务',
						implicit:false,
						chean: [
							{ name: '胡国庆',img:'https://source.iambuyer.com/cyy-yuanqu-gn-3.png' },
						]
					}
				]
			},
			height:'',
			show:true,
			more:'-1',
			arr:[],
		};
	},
	mounted() {
		this.calculation()
	},
	methods: {
		xianshi(e){
			var _this=this
			if(this.list.son[e].implicit==true){
				_this.list.son[e].implicit=false
				_this.$nextTick(() => { 
					_this.calculation()
				})
				// setTimeout(function(){
				// 	_this.calculation()
				// },10)
			}else if(this.list.son[e].implicit==false){
				_this.list.son[e].implicit=true
				_this.$nextTick(() => {
					_this.calculation()
				})
				
			}
		},
		addkaiguan(){
			this.list.implicit=!this.list.implicit
			this.show=!this.show
		},
		calculation(){
			var _this=this
			uni.getSystemInfo({
				success: function(res) {
					let chan=_this.list.son.length
					//获取到所有的class为.hea2的元素
					var info = uni.createSelectorQuery().selectAll('.hea2');
					info.boundingClientRect(function(data) {
						_this.arr=[]
						data.map((item,index)=>{//把所有.hea2的高度放到一个数组中
							_this.arr.push(item.height)
						})
						let num = eval(_this.arr.join('+'))//计算出所有.hea2的高度的和
						let end = _this.arr[_this.arr.length-1]//减去最后一个.hea2的高度
						let math = num-end+40+10*_this.arr.length-1  //算出所需要的线条的高度
						_this.height = math //最后赋值
						console.log(_this.height)
					}).exec();
				}
			});
		}
	}
};
</script>

<style>
.box {
	width: 50%;
	min-height: 1000rpx;
	margin: 0 auto;
}
.hea {
	position: relative;
	min-height: 60rpx;
}
.Line {
	position: absolute;
	border-left: 1px dashed red;
	width: 4rpx;
	/* height: 500rpx; */
	left: 20rpx;
	top: 55rpx;
}
.image {
	width: 50rpx;
	height: 50rpx;
}
.images {
	width: 50rpx;
	height: 50rpx;
	border-radius: 50%;
	vertical-align: middle;
	margin-top: -5px;
}
.span1 {
	margin-left: 20rpx;
}
.span2 {
	margin-left: 20rpx;
	font-size: 15px;
}
.hea2 {
	min-height: 90rpx;
	margin-left: 80rpx;
	margin-top: 20rpx;
	line-height: 90rpx;
	position: relative;
}
.hea3 {
	min-height: 60rpx;
	margin-left: 80rpx;
	margin-top: 20rpx;
	line-height: 60rpx;
	position: relative;
}
.widthss {
	position: absolute;
	width: 60rpx;
	border-top: 1px dashed red;
	left: -60rpx;
	top: 40rpx;
}
</style>
