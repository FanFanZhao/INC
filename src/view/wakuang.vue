<template>
    <div id="legaltrade-box" class="bPart">
        <div class="flex alcenter pdb10 bdb fColor2 ft14 border_cor">
            <span class="flex1">时间</span>
            <span class="flex1 tc">状态</span>
            <span class="flex1 tc">锁仓天数</span>
            <span class="flex1 tc">剩余天数</span>
            <span class="flex1 tr">锁仓金额</span>
        </div>
        <div class="mt10 plr10">
            <div class="flex pdtb10 ft14" v-for="item in datas" v-if="datas.length">
                <span class="flex1">{{item.create_date}}</span>
                <span class="flex1 tc">{{item.status_name}}</span>
                <span class="flex1 tc">{{item.days}}</span>
                <span class="flex1 tc">{{item.time_out}}</span>
                <span class="flex1 tr">{{item.money}}</span>
            </div>
        </div>
        <div class="more tc ft14 mt10" @click="getMore" v-if="datas.length">{{more}}</div>
   
        <div v-else class="nomore tc ft14 mt10">暂无更多</div>
    </div>
</template>
<script>
export default {
    name:'wakuang',
    data(){
        return{
            datas:[],
            page:1,
            more:'加载更多'
        }
    },
    created(){
      this.getList();
      this.token = window.localStorage.getItem("token") || "";
    },
    mounted(){

    },
    methods:{
        getList(){
            var i = layer.load();
            this.token = window.localStorage.getItem("token") || "";
            this.$http({
                url: "/api/lock_position/mine",
                method: "get",
                params:{page:this.page},
                headers:{Authorization:this.token}
            }).then(res => {
                layer.close(i);
                console.log(res)
                if (res.data.type == "ok") {
                    if( res.data.message.data.length != 0){
                        this.more = '加载更多'
                        this.datas = this.datas.concat(res.data.message.data);
                    }else{
                        this.more = '没有更多了'
                    }   
                }
            });
        },
        getMore(){
            this.page++;
            this.more = '加载中...'
            this.getList();
        }
    }
}
</script>
<style lang='scss'>
	#legaltrade-box {
		width: 85%;
		margin: 5px auto;
        padding: 40px;
    }
    .bdb{
        border-bottom: 1px solid #3c3c3c;
    }
    .more{
        cursor: pointer;
    }
</style>


