<template>
    <div class="inp_wrap">
        <div class="inp-item">
            <div>姓名</div>
            <input type="text" class="请输入姓名" v-model="name">
        </div>
        <div class="inp-item">
            <div>微信账号</div>
            <input type="text" class="请输入微信账号" v-model="wechat">
        </div>
        <div>
            <p class="ewm_title">收款二维码</p>
            <div class="flex alcenter ewm_wrap">
                <img class="ewmImg" :class="{'opacity':isshow}" :src="src1" >
                <div class="addEwm">
                <span class="addText">添加二维码</span>
                <input class="add" type="file" id="file" accept="image/*" name="file" @change="file1">
                </div>
            </div>
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
            src1:'',
            name:'',
            wechat:'',
            isshow:true
        }
    },
    created(){
        this.token=localStorage.getItem('token');
        this.getInfo();
    },
    methods:{
        file1(){
            var that = this;
            var reader = new FileReader();
            reader.readAsDataURL(event.target.files[0]); 
            reader.onload = function(e){
                console.log(e)
                that.src1=e.target.result
            } 
            var formData = new FormData();
            formData.append("file", event.target.files[0]); 
            $.ajax({
                url: '/api/'+'upload',
                type: 'post',
                data: formData,
                processData: false,
                contentType: false,
                beforeSend: function beforeSend(request) {
                    request.setRequestHeader("Authorization", that.token);
                },
                success: function (msg) {
                    console.log(msg)
                    that.src1=msg.message;
                    that.isshow = false

                }
            });     

        },
        add(){
           
            if(this.name == ''){
                layer.msg('请填写姓名');
                return;
            }
            if(this.wechat == ''){
                layer.msg('请填微信账号');
                return;
            }
            if(this.src1 == ''){
                layer.msg('请填添加收款二维码');
                return;
            }
             var i = layer.load();
           this.$http({
                url: '/api/'+'user/cash_save',
                method:'post',
                data:{
                    wechat_account:this.wechat,
                    wechat_name:this.name,
                    wechat_qr_code:this.src1
                },  
                headers: {'Authorization':  this.token}    
            }).then(res=>{
                    layer.close(i);
                    console.log(res);
                    layer.msg(res.data.message);
                })
        },
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
                        this.name = res.data.message.wechat_name;
                        this.wechat = res.data.message.wechat_account;
                        this.src1 = res.data.message.wechat_qr_code;
                        this.isshow = false
                    }
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
    .addEwm{
      position: relative;
  }
  .add{
      position: absolute;
      top: 0;
      left: 0;
      z-index: 99;
      opacity: 0;
      cursor: pointer;
  }
  .opacity{
     opacity: 0;
  }
  .addText{
      cursor: pointer;
  }
</style>


