<template>
    <div class="inp_wrap">
        <div class="inp-item">
            <div>姓名</div>
            <input type="text" class="请输入姓名" v-model="name">
        </div>
        <div class="inp-item">
            <div>开户行名称</div>
            <input type="text" class="请输入开户行名称" v-model="bankName">
        </div>
        <div class="inp-item">
            <div>银行卡账号</div>
            <input type="text" class="请输入银行卡账号" v-model="bankAccount">
        </div>
         <div class="btn bgRed" @click="add">
            确认
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            name:'',
           bankName:'',
           bankAccount:''
        }
    },
    created(){
        this.token=localStorage.getItem('token');
        this.getInfo();
    },
    methods:{
        //获取信息
        getInfo(){
            var i = layer.load();
            this.$http({
                url: '/api/'+'user/cash_info',
                method:'post',
                data:{},  
                headers: {'Authorization':  this.token}    
            }).then(res=>{
                    layer.close(i);
                    console.log(res);
                    if(res.data.type == 'ok'){
                        this.name = res.data.message.real_name;
                        this.bankName = res.data.message.bank_name;
                        this.bankAccount = res.data.message.bank_account;
                    }
                })
        },
        add(){
            if(this.name == ''){
                layer.msg('请填写姓名');
                return;
            };
            if(this.bankName == ''){
                layer.msg('请填写开户行名称');
                return;
            };
            if(this.bankAccount == ''){
                layer.msg('请填写银行卡账号');
                return;
            };
            var i = layer.load();
           this.$http({
                url: '/api/'+'user/cash_save',
                method:'post',
                data:{
                    real_name:this.name,
                    bank_name:this.bankName,
                    bank_account:this.bankAccount
                },  
                headers: {'Authorization':  this.token}    
            }).then(res=>{
                    layer.close(i);
                    console.log(res);
                    layer.msg(res.data.message);
                })
        }
    }
}
</script>
<style lang="scss" scoped>
.ewm_title{
    width: 160px;
    text-align: center;
    padding: 8px 0;
}
    .inp_wrap{
        width: 600px;
        margin: 0 auto;
    }
    .inp-item {
    position: relative;
    margin-bottom: 20px;
    border: 1px solid #ddd;
    padding-left: 160px;
    height: 42px;
    > div {
      position: absolute;
      top: 0;
      left: 0;
      border-right: 1px solid #ddd;
      width: 160px;
      height: 40px;
      line-height: 40px;
      text-align: center;
      background: #f8f8f8;
    }
    > input {
      display: block;
      padding: 0 20px;
      width: 100%;
      line-height: 40px;
    }
  }
  .ewmImg{
      width: 150px;
      height: 150px;
      margin-right: 100px;
      opacity: 0;
  }
  .ewm_wrap{
      padding: 40px;
      cursor: pointer;
  }
  .btn {
    margin: 30px auto;
    width: 400px;
    line-height: 40px;
    text-align: center;
    background: #d45858;
    color: #fff;
    cursor: pointer;
  }
</style>


