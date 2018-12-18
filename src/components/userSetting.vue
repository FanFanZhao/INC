<template>
    <div id="user-setting" class="flex">
        <div class="setting-l bPart">
            <div class="setting-item flex column">
                <!-- <div class="item-title">收款方式</div> -->
                <!-- <router-link to="/userSetting">收款方式</router-link> -->
                <router-link class="tc" to="/userSetting">支付宝绑定</router-link>
                <router-link class="tc" to="/bindWechat">微信绑定</router-link>
                <router-link class="tc" to="/bindBank">银行卡绑定</router-link>
            </div>
        </div>
        <div class="setting-r bPart">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
export default {};
</script>

<style lang='scss'>
#user-setting {
    margin-top: 5px;
    min-height: 700px;
    width: 85%;
    margin: 5px auto;
  > .setting-l {
      padding: 20px;
      line-height: 40px;
    width: 23%;
    border-right: 1px solid #ddd;
    .router-link-active{
        color:#d45858;
    }
  }
  .setting-r {
      padding: 20px;
    width: 77%;
  }
}
</style>
