<template>
	<div class="home">
		<div class="main">
			<div class="main-l fl bMain" >
				<div class="sidebar">
          <market></market>
        </div>
				<div class="notice_box bPart" style="margin-top:5px">
        <notice></notice>
      </div>
			</div>
			<div class="main-r" style="background:none;">
				<div class="chart_wrap flex">
          <!-- <chart></chart> -->
          <div style="min-width:67%">
            <!--<kline></kline>-->
			      <tv></tv>
          </div>
          <div class="deadl-wrap part-r bPart" style="width:33%;margin-left:5px;">
						<exchange></exchange>
					</div>
        </div>
				<div class="parts  flex between" >
					<div class="trade-wrap part-l bPart">
						<trade></trade>
					</div>
					<div class="deatil_box bPart" style="width:33%;margin-left:5px;">
          <complete></complete>
        </div>
				</div>
        <div class="entrust_box bPart" >
          <entrust></entrust>
        </div>
        <!-- <div class="entrust_box">
          <hisentrust></hisentrust>
        </div> -->
        <!-- <div class="deatil_box" style="background: #18184c;">
            <detail></detail>
        </div> -->

				<!-- <div class="currency_box">
          <currency></currency>
        </div> -->
				<!-- <div class="depth-map flex btween" >
					<deal></deal>
				</div> -->

			</div>
		</div>

	</div>
</template>

<script>
import indexHeader from "@/view/indexHeader";
import notice from "@/components/noticeList";
import deal from "@/view/deal";
import exchange from "@/view/exchange";
import market from "@/view/market";
import trade from "@/view/trade";
import chart from "@/view/chart";
import entrust from "@/view/entrust2";
import hisentrust from "@/view/hisentrust";
import detail from "@/view/detail";
import currency from "@/view/currency";
import complete from "@/view/complete";
import tv from '@/view/tv'
export default {
  name: "dealCenter",

  components: {
    indexHeader,
    load:1,
    notice,
    deal,
    exchange,
    market,
    trade,
    chart,
    entrust,
    hisentrust,
    detail,
    currency,
    complete,
	  tv
  },
  data() {
    return {};
  },
  created() {
    this.address = localStorage.getItem("address") || "";

    // console.log(this.address)
  },
  mounted(){
    // console.log('wejdewhbewjdbewjdhbcwj')
    // if(this.load==1){
    //   console.log(1111)
    //   window.location.reload();
    //   this.load=2;
    // }

  }
};
</script>

<style scoped lang="scss">
.home {
  background: none!important;
  .main {
      overflow: hidden;
    .main-l {
      width: 350px;
      height: 100%;
      margin: 5px 0  0 0;
      > div {
        // background: #fff;
        border: 1px solid #C8DAE2;
        // background: #18184c;
        margin: 0 0 5px 5px;
      }
    }
    .main-r {
      margin-left: 355px;
      border-left: 1px solid #ccc;
      padding: 0 5px 5px 0px;
      .parts {
        margin-top: 5px;
        max-height: 530px;

        > .part-l {
          // background: #18184c;
          min-width: 67%;
          padding-bottom: 20px;
        }
        > .part-r {
          width: 33%;
          > div {
            margin-left: 5px;
            // background: #18184c;
            height: 100%;
            padding-bottom: 20px;
          }
        }
      }
      >div{
        //  background: #18184c;
         margin-top: 5px;
      }
      .depth-map {
        max-height: 500px;
        margin-top: 5px;
      }
    }
  }
}
</style>
