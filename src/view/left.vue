<template>
	<div class="leftNav ft16 bPart">
		<ul class="fColor1">
			<li v-for="(item,index) in array" :class="index == curActive ? 'active' :''" @click="goto(index,item.page)">
				<img :src="index == curActive ? item.src2 : item.src1"/>
				<span>{{item.text}}</span>
			</li>
		</ul>
	</div>
</template>
<script>
	export default {
		name: "left",
		data() {
			return {
				curActive:0,
				array:[
					// {
					// 	src1: require("@/assets/images/icon_mine.png"),
					// 	src2: require("@/assets/images/icon_mine_s.png"),
					// 	text: '账户中心',
					// 	page:'accountCenter',
					// 	children:['recharge']
					// },
					{
						src1: require("@/assets/images/icon_account_settings.png"),
						src2: require("@/assets/images/icon_account_settings_s.png"),
						text: '账户设置',
						page:'accountSet'
					},
					// {
					// 	src1: require("@/assets/images/icon_mine.png"),
					// 	src2: require("@/assets/images/icon_mine_s.png"),
					// 	text: '身份认证',
					// 	page:'authentication',
					// },
				],
			}
		},
		created(){
			let name = this.$route.name;
			console.log(name)
			this.curActive = this.array.findIndex( val => val.page === name || (val.children && val.children.includes(name))) ;
		},
		mounted(){
			this.bus.$on("nav_name", name =>{
				this.curActive = this.array.findIndex( val => val.page === name || (val.children && val.children.includes(name))) ;
			})
		},
		methods: {
			goto(index,name) {
				this.curActive=index;
				this.$router.push({name: name, params: {lang: this.lang}})
			},

		}
	};
</script>
<style lang="scss" scoped>
	.leftNav{
		margin-right: 20px;
		padding: 4px;
		width: 210px;
		float: left;
		min-height: 800px;
		ul{
			height:820px;
			li{
				height:54px;
				line-height:54px;
				padding-left:38px;
				margin-bottom:4px;
				cursor:pointer;
				img{
					margin-right:12px;
					margin-top:-4px;
					width:20px;
					vertical-align: middle;
				}
			}
			.active{
				color:rgb(86, 151, 244);
				// background-color: rgb(39, 49, 66);
				border-bottom: none;
				display: block;
			}
		}
	}
</style>


