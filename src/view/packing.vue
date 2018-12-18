<template>
    <div class="paking_wrap bPart">
        <p class="mb10 select" >选择天数：</p>
        <ul class="ul">
            <li class="tc ft14 border_cor" :class="{actives:index == idx}" @click="selDay(item,index)"  v-for="(item,index) in days" :key="index">{{item}}天</li>
        </ul>
        <div class="flex center">
        <input class="inp mt10 mb10 border_cor bMain" v-model="money" placeholder="请输入数量" type="number" />
        </div>
        <div class="tc pack" @click="pack">锁仓</div>
        <div class="text_wrap mt10 mb10">
        <p class="ft16 white mb10">锁仓说明：</p>
        <p class="ft14 zh_text mb10">inc 是基于以太2.0开源的P2P软件产生的数字货币，依据全世界的计算机运算一组方程式开源码。

                而是使用遍布整个P2P网络节点的分布式数据库来记录货币的交易，并使用密码学的加密设计来确保它流通的各个环节的安全性。

                INC的创造和转让基于一种开源加密协议，该系统让遍布整个P2P对等网络的各用户端节点达成网络协议！</p>
        <p class="ft14 en_text">
                Inc is the digital currency generated based on Ether 2.0 open source P2P software

                According to the world's computer operation a set of equations open source

                Instead, use a distributed database spread across the entire P2P network node to record currency
                transactions.

                And use cryptographic encryption design to ensure the security of all aspects of its circulation

                The creation and transfer of INC is based on an open source encryption protocol

                The system allows each client node throughout the P2P peer-to-peer network to reach network protocols.
            </p>
        </div>
    </div>
</template>
<script>
export default {
     data(){
         return{
             days:[15,46,120,280],
             idx:'a',
             money:'',
             day:''
         }
     },
      created() {
    this.token = window.localStorage.getItem("token") || "";
      },
     methods:{
         selDay(item,index){
             this.idx = index;
             this.day = item;
         },
         pack(){
             if(this.day == ''){
                 layer.msg('请选择天数');
                 return;
             }
             if(this.money == ''){
                 layer.msg('请输入数量');
                 return;
             }
             
             this.$http({
                url:'/api/'+'lock_position/add',
                method:'POST',
                data:{money:this.money,days:this.day},
                headers:{Authorization:this.token}
            }).then(res => {
                console.log(res);
               layer.msg(res.data.message);
                if(res.data.type == 'ok'){
                    this.$router.push('/wakuang');
                }      
            })
         }
     }
}
</script>
<style scoped>
    .paking_wrap{
        width: 85%;
        margin: 5px auto;
        padding: 40px;
    }
    .ul li{
        padding:8px;
        width:50%;
        border:1px solid;
        margin: 20px auto;
        cursor: pointer;
    }
    .select{
        width: 50%;
        margin: 10px auto;
    }
    .inp{
        width: 50%;
        padding: 8px;
        margin: 0 auto;
        background: none;
        border: 1px solid;
        color: #ccc;
    }
    .pack{
        width: 50%;
        padding: 8px;
        background: #cc4951;
        color: #eee;
        margin: 10px auto;
        margin-top: 30px;
        cursor: pointer;
    }
    .text_wrap{
        width: 80%;
        margin: 20px auto;
    }
    .zh_text,.en_text{
        line-height: 1.8;
    }
    .actives{
        color: #cc4951;
        background: #262a42
    }
</style>
