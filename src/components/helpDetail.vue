<template>
    <div class="noticeDetail bMain">
        <indexHeader></indexHeader>
        <div class="account-wrap bPart" style="background:##14143f">
            <div class="account" style="background:##14143f">
                <div>
                    <div class="back-nav  ft16"> 
                        <span class="fr  curPer" @click="goBefore">&lt;&lt;返回</span>
                    </div>
                    <div class="nav-after"></div>
                </div>
                <div class="account-content" style="background:##14143f">
                    <div class="detailBig" style="background:##14143f">
                        <div class="mb30 clear">
                            <span class=" w90 fl tc">{{title}}</span>
                        </div>
                        
                        <div class="detailContent " style="background:##14143f">
                            <p v-html="content" ref="con"></p>
                        </div> 
                        <div class=" mt40">
                            <!-- <p class="tr">{{abstract}}</p> -->
                            <p class="tr mt5">{{update_time}}</p>
                        </div>
                    </div>
                </div>
             </div>
        </div>
    </div>
</template>
<script>
import indexHeader from '@/view/indexHeader'
export default {
    name:'helpDetail',
    components:{indexHeader},
    data (){
        return{
            title:'',
            content:'',
            abstract:'',
            update_time:'',
            url_img:''
        }
    },
    created(){
        this.id = this.$route.query.id;
        var id = this.id;
        this.$http({
            url:  '/api/news/detail?'+'id='+id,
            method:'post',
            headers: { 'Authorization': window.localStorage.getItem('token') }
        }).then(res=>{
            res = res.data;
            if(res.type  === 'ok'){
                console.log(res.message)
                this.title=res.message.title;
                this.content=res.message.content;
                this.abstract=res.message.abstract;
                this.update_time=res.message.update_time;
                this.url_img=res.message.thumbnail;
                this.setProperty();
            }else{
                layer.msg(res.message);
            }
        }).catch(error=>{
            console.log(error)
        })

    },
    mounted(){
        // var tags=this.$refs.con.getElementsByTagName('p');
        // console.log(tags)
        // for(var i=0;i<tags.length;i++){
        //     console.log(tags)
        //     console.log(tags[i])
        //     tags[i].style.background='transparent'
        // }
    },
    methods:{
        goBefore(){
            this.$router.back(-1);
        },
       setProperty(){
            var tags=document.getElementsByTagName('p');
            HTMLCollection.prototype.forEach=function(callback){
                    [].slice.call(this).forEach(callback);
            };
            tags.forEach(function(e, i){
                    e.style.backgroundColor='#666 !important'
            });
            
            
       }
    }
}
</script>
<style lang="scss" scoped>
.noticeDetail{
    .back-nav{
        width: 100%;
        overflow: hidden;
    }
    .account-wrap{
        width: 85%;
        margin: 5px auto;
        // background: url(../assets/images/account_center_bg.jpg) no-repeat;
        // background-size: cover;
        .account {
            width: 1500px;
            margin: 0 auto;
            padding-top: 30px;
            overflow: hidden;
            min-height: 880px;
            .nav-after{
                display: block;
                height: 10px;
                // background-color: #14143f;
            }
            .account-content {
                width: 100%;
                min-height: 750px;
                // background-color: #18184c;
                .detailBig{
                    padding: 48px 46px 20px;
                    .mb30{
                        margin-bottom: 30px;
                    }
                    .detailContent{
                        line-height: 26px;
                        p{
                          &>*{
                            background-color: transparent!important;
                           }  
                        }
                       
                    }
                    .mt5{
                        margin-top: 5px;
                    }
                }

            }

        }
    }
}
</style>



