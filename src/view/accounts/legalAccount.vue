<template>
    <div class="main   clr-part bPart">
        <!-- <p class="legal_name">{{legal_name}}</p> -->
        <div class="legalAccount_msg flex between bg-part bPart" style="padding:20px 30px">
          <div>
              <p class="ft12 msg_title">可用（{{legal_name}}）</p>
              <p style="color:#EF5E41">{{legal_balance}}</p>
          </div>
          <div>
              <p class="ft12 msg_title">冻结（{{legal_name}}）</p>
              <p style="color:#4A95F1">{{lock_legal_balance}}</p>
          </div>
          <div>
              <p class="ft12 msg_title">折合（CNY）</p>
              <p style="color:#2EDB99">{{legal_price}}</p>
          </div>
        </div>
        <div class="transfer bg-part bPart" style="padding:20px 30px">
            <div class="direction flex">
                <div>{{transferPms.type == 1?'法币':'交易'}}</div>
                <img src="../../assets/images/transfer.png" alt="" @click="transferPms.type == 1?transferPms.type =2:transferPms.type =1">
                <div>{{transferPms.type == 1?'交易':'法币'}}</div>
            </div>
            <div class="flex">
                <span>选择币种：</span>
                <select name="" id="" style="padding:3px 16px" class="input_bg" ref="select" @change="selectChange">
                    <option v-for="(item,index) in coins" :key="index" :value="item.currency">{{item.currency_name}}</option>
                </select>
            </div>
            <div class="flex">
                <span>划转数量：</span>
                <input type="numer" v-model="transferPms.number" class="input_bg">
            </div>
            <button type="button" class="curPer" :disabled='transferPms.number == ""' @click="transfer">划转</button>
        </div>
        <div class="rec_wrap rec_wrap">
           <p class="rec_title flex between " style="padding:20px 30px">
               <span>财务记录</span>
               <!-- <span class="all">全部</span> -->
           </p>
           <p class="list_title flex around" style="padding:15px 30px;">
               <span class="ft14 flex1">数量</span>
               <span class="ft14 flex1 tc">记录</span>
               <span class="ft14 flex1 tr">时间</span>
           </p>
           <div class="log_wrap bPart border_cor">
           <ul class="" style="padding:20px 30px">
              <li class="flex around ft12" v-for="(item,index) in recData" :key="index">
                  <span class="flex1 ft12 ptb ">{{item.value}}</span>
                  <span class="flex1 ft12 ptb tc">{{item.info}}</span>
                  <span class="flex1 ft12 ptb tr">{{item.created_time}}</span>
              </li>
           </ul>
           <p class="ft12 no_rec fColor2" style="text-align:center;padding:20px 30px" v-show="recData.length == 0" >暂无记录</p>
           <p class="ft12 no_rec fColor2" style="text-align:center;padding:20px 30px" v-show="recData.length != 0" @click="more">{{moreLog}}</p>
           </div>
        </div>
        <!-- <router-link tag="p" class="huazhuan redBg" to='/transferLegal'>划转</router-link> -->
    </div>
</template>
<script>
export default {
  data() {
    return {
      legal_name: "",
      legal_balance: "",
      lock_legal_balance: "",
      legal_price: "",
      recData: [],
      moreLog: "加载更多",
      page: 1,
      currencyId: "",
      coins: [],
      transferPms: { number: "", type: 2 }
    };
  },
  created() {
    this.token = localStorage.getItem("token") || "";
    if(this.token){
      this.currencyId = this.$route.query.currency_id;

      this.init();
      this.getLog();
      this.getCoins();
    }
  },
  mounted() {
    
  },
  methods: {
    selectChange(){
      this.currencyId = this.$refs.select.value;
      this.init();
      this.page = 1;
      this.getLog(true)
    },
    getCoins() {
      var load = layer.load();
      this.token = window.localStorage.getItem("token") || "";
      this.$http({
        url: "/api/wallet/list",
        method: "post",
        headers: { Authorization: this.token }
      }).then(res => {
        layer.close(load);
        if (res.data.type == "ok") {
          this.coins = res.data.message.legal_wallet.balance;
        }
      });
    },
    init() {
      var load = layer.load();
      var that = this;
      this.$http({
        url: "/api/" + "wallet/detail",
        method: "post",
        data: {
          currency: this.currencyId,
          type: "legal"
        },
        headers: { Authorization: that.token }
      })
        .then(res => {
          layer.close(load);
          console.log(res);
          if (res.data.type == "ok") {
            var msg = res.data.message;
            that.legal_name = msg.currency_name;
            that.legal_balance = msg.legal_balance;
            that.lock_legal_balance = msg.lock_legal_balance;
            that.legal_price = msg.cny_price;
          } else {
            layer.msg(res.message);
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    //获取记录
    getLog(refresh) {
      var load = layer.load();
      if(refresh){
        this.page = 1;
      }
      this.$http({
        url: "/api/wallet/legal_log",
        method: "post",
        data: {
          type: "1",
          currency: this.currencyId,
          page: this.page
        },
        headers: { Authorization: this.token }
      }).then(res => {
        layer.close(load);
        console.log(res);
        console.log(res.data.message.list);
        if (res.data.type == "ok") {
          console.log(res);
          if(refresh){
            this.recData = res.data.message.list;
          } else {

            this.recData = this.recData.concat(res.data.message.list);
          }
          if (res.data.message.list.length != 0) {
            this.moreLog = "加载更多";
          } else {
            this.moreLog = "没有更多记录了";
          }
        }
      });
    },
    transfer(){
        
        if(this.transferPms.number == ''){
            layer.msg('请输入划转数量');return;
        } else {
            let data = {};
            data.number = this.transferPms.number;
            
            data.currency_id = this.$refs.select.value;
            data.type = this.transferPms.type;
            // console.log(data);return;
            var load = layer.load();
            this.$http({
                url:'/api/wallet/change',
                method:'post',
                data:data,
                headers: { 'Authorization': this.token }
            }).then(res => {
                layer.close(load);
                console.log(res);
                this.transferPms.number = '';
                layer.msg(res.data.message)
                if(res.data.type == 'ok'){
                    this.init();
                    this.getLog();
                }
            })
        }
    },
    //加载更多
    more() {
      this.page++;
      this.moreLog = "加载中...";
      this.getLog();
    }
  }
};
</script>
<style scoped lang='scss'>
.legal_name {
  /* background: #1b1e2e; */
  padding: 5px 0;
}
.msg_title {
  /* color: #61688a; */
}
.main {
}
.transfer .flex {
  margin: 16px 0;
  line-height: 40px;
  span {
    width: 100px;
  }
  select {
    border-radius: 2px;
  }
  input {
    padding: 0 14px;
    border-radius: 2px;
    line-height: 39px;
    border: 1px solid #ccc;
  }
}
.transfer button {
  padding: 10px 60px;
  background: #563bd1;
  color: #fff;
  border-radius: 2px;
  margin-top: 20px;
}
.direction {
  padding: 0 20px;
  line-height: 44px;
  background: #fff5dc;
  margin-top: 10px !important;
  img {
    margin: 0 44px;
    cursor: pointer;
  }
}
.legalAccount_msg {
  /* background: #1b1e2e; */
  padding: 6px 0;
}
.legalAccount_msg div p:first-child {
  margin-bottom: 10px;
  font-size: 18px;
}
.legalAccount_msg div p:last-child {
  font-size: 24px;
}
.rec_wrap {
  margin-top: 20px;
  margin-bottom: 30px;
}

.all {
  /* color: #61688a; */
}
.list_title {
  padding: 10px 0;
  /* background: #1b1e2e; */
}
.huazhuan {
  text-align: center;
  margin-top: 60px;
  background: #d45858;
  padding: 8px;
}
.huazhuan:hover {
  cursor: pointer;
}
.ptb {
  padding: 8px 0;
}
.no_rec {
  cursor: pointer;
}
.log_wrap {
  height: 480px;
  overflow: auto;
  border: 1px solid #ededed;
}
.log_wrap::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 2px; /*高宽分别对应横竖滚动条的尺寸*/
  height: 8px;
}
.log_wrap::-webkit-scrollbar-thumb {
  /*滚动条里面小方块*/
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  background: #9e9898;
}
.log_wrap::-webkit-scrollbar-track {
  /*滚动条里面轨道*/
  -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  background: #ededed;
}
</style>
