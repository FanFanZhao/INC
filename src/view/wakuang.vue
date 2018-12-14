<template>
    <div id="legaltrade-box">
        <div class="flex alcenter pdb10 bdb">
            <span class="flex1"> 数量</span>
            <span class="flex1 tc">记录</span>
            <span class="flex1 tr">时间</span>
        </div>
        <div class="mt10 plr10">
            <div class="flex pdtb10" v-for="item in datas" v-if="">
                <span class="flex1">{{item.value}}</span>
                <span class="flex1 tc">{{item.info}}</span>
                <span class="flex1 tr">{{item.created_time}}</span>
            </div>
        </div>
        <!-- <div class="more tc" @click="getList()" v-if="datas.length">加载更多</div>
   
        <div v-else class="nomore">暂无更多</div> -->
    </div>
</template>
<script>
export default {
    name:'wakuang',
    data(){
        return{
            datas:[],
        }
    },
    created(){
      this.getList();
    },
    mounted(){

    },
    methods:{
        getList(){
            this.token = window.localStorage.getItem("token") || "";
            this.$http({
                url: "/api/return_bkb",
                method: "get",
                data:{},
                headers:{Authorization:this.token}
            }).then(res => {
                if (res.data.type == "ok" && res.data.message.length != 0) {
                    this.datas = res.data.message;
                }
            });
        }
    }
}
</script>
<style lang='scss'>
	#legaltrade-box {
		width: 1200px;
		margin: 30px auto;
    }
    .bdb{
        border-bottom: 1px solid #3c3c3c;
    }
</style>


