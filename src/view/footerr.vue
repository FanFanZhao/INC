<template>
    <div class="footer_wrap ">
        <!-- <div class="left ">
            <h1 class="mb15 logo">BKB Global</h1>
            <p class="mb15">全球领先的数字资产交易平台</p>
            <div class="icon_img">
                <i class="iconfont icon-weibo"></i>
                <i class="iconfont icon-weibo"></i>
                <i class="iconfont icon-weibo"></i>
                <i class="iconfont icon-weibo"></i>
            </div>
            <p class="mt50">© 2013-2018 BKB Global</p>
        </div> -->
        <div class="">
          <div class="footer_box">
              <div class="flex mb10">
                <p class="fl tl foot_title ft16 flex1 " v-for="item in titleList">{{item}}</p>
              </div>
              <div class="flex">
                  <div v-for="item in conList" class="flex1 "><router-link :to="itm.src" class="con_a mb10 ft12" v-for="(itm,index) in item.list" :key="index">{{itm.text}}</router-link></div>
                  <div class="flex1">
                    <p  class="con_a mb10 ft12" v-for="item in contact" >{{item}}</p>
                  </div>
                  <div class="flex1 "><img src="../assets/images/join_code.jpg" alt="" class="ewm"></div>
              </div>
              
            </div>
        </div>
    </div>
</template>
<script>
export default {
  // , "关注我们"
  data() {
    return {
      titleList: ["关于我们", "帮助中心", "联系我们","关注我们"],
      conList: [
        {
          list: [{ text: "隐私条款", src: "/help" },{ text: "注册协议", src: "/help" },{ text: "币种介绍", src: "/help" }]
        },
        {
          list: [{ text: "账户管理", src: "/help" },{ text: "交易管理", src: "/help" },{ text: "充值提现", src: "/help" },{ text: "费用标准", src: "/help" }]
        },
      ],
      contact: [
          "QQ群号：317660286","客服微信：coinbkb", "客服邮箱：coinbkb@sina.com"
        ],
    };
  }
};
</script>
<style scoped>
.footer_wrap {
  background: #14143f;
  color: #6b80ae;
  overflow: hidden;
  padding: 50px 80px;
 
}
.footer_box{
  width: 100%;
  margin: 0 auto;
}
.footer_wrap >div{
    width: 1200px;
    margin:  0 auto;
  }
.foot_title,.con_a{
  display: inline-block;
  width: 180px;
  text-align: left;
}
.con_a{
    color: #aabdbc;
}
.con_a:hover{
    color: #d45858;
}
.logo{
    color: #6b80ae;
}
.ewm{
  width: 100px;
}
</style>


