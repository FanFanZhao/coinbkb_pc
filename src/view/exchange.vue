<template>
    <div class="exchange clr-part flex column">
		<!-- <div class="title fColor1">交易所</div> -->
        <div class="content bg-part mb10 flex1">
            <div class="new_price bdr-part">
                <span class="ft14">{{$t('center.newprice')}} {{newData}}{{legal_name}}</span>
            </div>
            <div class="exchange_title ft14 clear tc">
                <span>{{$t('center.direction')}}</span>
                <span>{{$t('price')}}({{legal_name}})</span>
                <span>{{$t('number')}}({{currency_name}})</span>
            </div>
            <ul class="list-item ft12 tc">
                <li v-show="isSocketIn" :class="['curPer','redColor','bg-hov',{'bg-evev':index%2 != 0}]" v-for="(out,index) in outlist" @click="price(out.price)">
                    <span >{{$t('center.sellout')}} {{outlist.length-index}}</span>
                    <span style="font-weight:600">{{out.price}}</span>
                    <span>{{out.number}}</span>
                </li>
                 <li v-show="!isSocketIn" :class="['curPer','redColor','bg-hov',{'bg-evev':index%2 != 0}]" v-for="(out,index) in outlist2" @click="price(out.price)">
                    <div v-if="index<7" class="flex1">
                      <span >{{$t('center.sellout')}} {{7-index}}</span>
                      <span style="font-weight:600">{{out[0]}}</span>
                      <span>{{out[1]}}</span>
                    </div>
                </li>
                <div class="line bdr-part"></div>
                 <li v-show="isSocketOut" class="curPer ceilColor bg-hov" v-for="(buy,inde) in inlist"  @click="price(buy.price)">
                    <span>{{$t('center.buyin')}} {{inde+1}}</span>
                    <span style="font-weight:600">{{buy.price}}</span>
                    <span>{{buy.number}}</span>
                </li>
                 <li v-show="!isSocketOut" class="curPer ceilColor bg-hov" v-for="(buy,inde) in inlist2"  @click="price(buy.price)">
                    <div v-if="inde<7" class="flex1">
                      <span>{{$t('center.buyin')}} {{inde+1}}</span>
                      <span style="font-weight:600">{{buy[0]}}</span>
                      <span>{{buy[1]}}</span>
                     </div>
                </li>
            </ul>
            
        </div>
        <!--全站交易-->
        <div class="detail clr-part bg-part flex1">
        <div class="title  topshadow">
            <div class="inblock">
                <span>{{$t('center.alltrade')}}</span>
            </div>
        </div>
        <div class="content" style="overflow:hidden">
            <ul class="list-title fColor2 ft14 clear bdr-part">
                <li class="fl w12">{{$t('time')}}</li>
                <li class="fl w12">{{$t('center.direction')}}</li>
                <li class="fl w12">{{$t('price')}}</li>
                <li class="fl w12">{{$t('home.volume')}}</li>
            </ul>
            <div class="containers scroll" v-if="(deList.length>0 )|| (deList2.length>0)">
                <ul v-show="isSocketComplet" v-for="itm in deList" class="list-item color ft12">
                    <li class="clear flex clr-part bg-hov alcenter">
                        <span class=" ">{{itm.time}}</span>
                        <span :class="itm.way == 1?'red':'green'">{{itm.way == 1?$t('center.outsell'):$t('center.inbuy')}}</span>
                        <span :class="itm.way == 1?'red':'green'">{{itm.price}}</span>
                        <span class="">{{itm.number}}</span>
                    </li>
                </ul>
                <ul v-show="!isSocketComplet" v-for="itm in deList2" class="list-item color ft12">
                    <li class="clear flex clr-part bg-hov alcenter">
                        <span class=" ">{{itm.ts | timeToFormat}}</span>
                        <span :class="itm.direction == 'buy'?'red':'green'">{{itm.direction == 'buy'?$t('center.outsell'):$t('center.inbuy')}}</span>
                        <span :class="itm.direction == 'buy'?'red':'green'">{{itm.price}}</span>
                        <span class="">{{itm.amount}}</span>
                    </li>
                </ul>
            </div>
            <div class="no_data tc" v-else>
                <img src="../assets/images/nodata.png" alt="">
                <p class="fColor2 ft18">{{$t('nodata')}}</p>   
            </div>
        </div>
    </div>
	</div>
</template>

<script>
export default {
  name: "exchange",
  data() {
    return {
      outlist: [],
      inlist: [],
      outlist2: [],
      inlist2: [],
      deList:[], //全站交易
      deList2:[], //全站交易
      load: 1,
      newData: 0,
      currency_name: "",
      legal_name: "",
      currency_id: "",
      legal_id: "",
      isSocketIn:false,
      isSocketOut:false,
      isSocketComplet:false,
    };
  },
  created: function() {
    // var local_lid = window.localStorage.getItem("l_id"),
    //   local_cid = window.localStorage.getItem("c_id");
    

  },
  mounted(){
    var that = this;
    var Data = JSON.parse(window.localStorage.getItem("tradeData"));
       that.newData = Data.now_price;
    var localData=JSON.parse(window.localStorage.getItem('tradeData'))
    that.currency_id = localData.currency_id;
    that.legal_id = localData.legal_id;
    that.connect();
    that.connect2();
    eventBus.$on("toExchange0", function(data0) {
      that.currency_name = data0.currency_name;
      that.legal_name = data0.legal_name;
      that.currency_id = data0.currency_id;
      that.legal_id = data0.legal_id;
      console.log(Data.now_price)
      console.log(data0)
      console.log(data0.now_price)
      that.newData =  Data.now_price != undefined ? Data.now_price : data0.now_price; //初始最新价赋值
      that.buy_sell(that.legal_id, that.currency_id);
    });
    eventBus.$on("toExchange", function(data) {
      that.currency_name = data.currency_name;
      that.legal_name = data.legal_name;
      that.currency_id = data.currency_id;
      that.legal_id = data.legal_id;
      that.buy_sell(that.legal_id,that.currency_id);
    });
    eventBus.$on('toExchange_sign',function(ischange){
      that.isSocketIn=true;
      that.isSocketOut=true;
      that.isSocketComplet=true;
    })
  //接收market组件传来的最新价
  
    eventBus.$on("toexchangeNowprice", function(data) {
      console.log('88888888888888888888888888')
      console.log(data)
        that.newData = data || Data.now_price;
    });
    
    
  },
  sockets: {

  },
  filters:{
    timeToFormat(t){
      function add0(m){return m<10?'0'+m:m };
      var that=this;
      console.log(t);
      //shijianchuo是整数，否则要parseInt转换
      var time = new Date(t);
      var y = time.getFullYear();
      var m = time.getMonth()+1;
      var d = time.getDate()+1;
      var h = time.getHours()+1;
      var mm = time.getMinutes()+1;
      var s = time.getSeconds()+1;
      return y+'-'+add0(m)+'-'+add0(d)+' '+add0(h)+':'+add0(mm)+':'+add0(s);
 
    }
  },
  methods: {
    
    
    price(price) {
      eventBus.$emit("toPrice", price);
    },
    // 第一次默认最新价数据
    buy_sell(legal_id, currency_id) {
      console.log('555555555555555555555555555555')
      var i = layer.load();
      this.$http({
        url: "/api/" + "transaction/deal",
        method: "post",
        data: {
          legal_id: legal_id,
          currency_id: currency_id
        },
        headers: { Authorization: localStorage.getItem("token") }
      })
        .then(res => {
             console.log(res)
          layer.close(i);
          //  var len = this.outlist.length;
          if (res.data.type == "ok") {
            this.inlist = res.data.message.in;
            this.outlist = res.data.message.out;
            this.deList = res.data.message.complete
            // this.newData = res.data.message.last_price;
            console.log("-------1-------")
             console.log(this.inlist[0])
             var buy_price = 0
             if(this.inlist[0] != undefined){
                buy_price = this.inlist[0].price
             }
             var sell_price = 0
             console.log(this.outlist.length)
             if(this.outlist.length > 0){
                var len = this.outlist.length;
                if(this.outlist[len-1] != undefined){
                    sell_price = this.outlist[len-1].price
                }
             }

           
            var totradePrice = {
              buyPrice:buy_price,
              sellPrice: sell_price
            }
            
            console.log(totradePrice)
            console.log("-------2-------")
            setTimeout(() => {
              eventBus.$emit('totradePrice',totradePrice)
            }, 100);
          } else {
            layer.msg(res.data.message);
          }
        })
        .catch(error => {
          // console.log(error)
        });
    },
     connect_old() {
      // console.log(legal_id, currency_id);
      var that = this;
      console.log("socket");
      $socket.emit("login", this.$makeSocketId());
      that.$socket.on("market_trade", msg => {
        console.log(msg);
        console.log(msg.data);
        console.log(msg.symbol,that.currency_name+'/'+that.legal_name);

        if (msg.symbol == (that.currency_name+'/'+that.legal_name)) {
          var data=msg.data;
          for(var i in data){
            var temp=JSON.parse(data[i]);
            that.deList.unshift(temp);
          }
          console.log(data,'-------------------------------------------------------')
          //组件间传值
          // var newPrice = {
          //   newprice: msg.last_price,
          //   newup: msg.proportion,
          //   istoken: msg.token,
          //   yesprice: msg.yesterday,
          //   toprice: msg.today
          // };
          // setTimeout(() => {
          //   eventBus.$emit("toNew", newPrice);
          // }, 1000);
          
         
         
          var inData = JSON.parse(msg.in);
          var outData = JSON.parse(msg.out);
          console.log(inData);
          console.log(outData)
          if(msg.complete){
               var complete = JSON.parse(msg.complete);
          }     
          var len01 = inData.length;
          var len02 = outData.length;
          
          if (msg.legal_id == that.legal_id && msg.currency_id == that.currency_id) {
            console.log(msg.last_price)
            that.newData = msg.last_price;
            var priceData = {
             buyPrice:inData.length !=0?inData[0].price:'',
             sellPrice:outData.length !=0?outData[len02-1].price:''
          }
          if(len01 == 0){
            priceData.buyPrice = ''
          }
          if(len02 == 0){
            priceData.sellPrice = ''
          }
           eventBus.$emit("priceToTrade", priceData);
            that.inlist = inData;
            that.outlist = outData;
            that.deList = complete;
          }
        }
      });
    },
    connect() {
      // console.log(legal_id, currency_id);
      var that = this;
      console.log("socket");
      that.$socket.emit("login", this.$makeSocketId());
      that.$socket.on("market_trade", msg => {
        console.log(msg.symbol,that.currency_name+'/'+that.legal_name);
        if (msg.symbol == (that.currency_name+'/'+that.legal_name)) {
          
          var data=JSON.parse(msg.data);
          var mydata=[];
          if(data.length>0){
            that.isSocketComplet=false;
          }else{
            that.isSocketComplet=true;
          }
          for(var i=0;i<data.length;i++){
            that.deList2.unshift(data[i]);
           
            console.log(that.deList2)
          }
          var len=that.deList2.length;
           if(len>40){
              that.deList2.splice(39,len-40);
            }
          // console.log(that.deList2,'-------------------------------------------------------')
          //组件间传值
          // var newPrice = {
          //   newprice: msg.last_price,
          //   newup: msg.proportion,
          //   istoken: msg.token,
          //   yesprice: msg.yesterday,
          //   toprice: msg.today
          // };
          // setTimeout(() => {
          //   eventBus.$emit("toNew", newPrice);
          // }, 1000);
          
         
         
          // var inData = JSON.parse(msg.in);
          // var outData = JSON.parse(msg.out);
          // console.log(inData);
          // console.log(outData)
          // if(msg.complete){
          //      var complete = JSON.parse(msg.complete);
          // }     
          // var len01 = inData.length;
          // var len02 = outData.length;
          
          // if (msg.legal_id == that.legal_id && msg.currency_id == that.currency_id) {
          //   console.log(msg.last_price)
          //   that.newData = msg.last_price;
          //   var priceData = {
          //    buyPrice:inData.length !=0?inData[0].price:'',
          //    sellPrice:outData.length !=0?outData[len02-1].price:''
          // }
          // if(len01 == 0){
          //   priceData.buyPrice = ''
          // }
          // if(len02 == 0){
          //   priceData.sellPrice = ''
          // }
          //  eventBus.$emit("priceToTrade", priceData);
          //   that.inlist = inData;
          //   that.outlist = outData;
          //   that.deList = complete;
          // }
        }
      });
    }
    ,
    connect2() {
      // console.log(legal_id, currency_id);
      var that = this;
      console.log("socket");
      that.$socket.emit("login", this.$makeSocketId());
      that.$socket.on("market_depth", msg => {
        console.log(msg);
        console.log(msg.data);
        console.log(msg.symbol,that.currency_name+'/'+that.legal_name);

        if (msg.symbol == (that.currency_name+'/'+that.legal_name)) {
          
          // 买盘
          var buys=JSON.parse( msg.bids);
          console.log(buys);
          // 卖盘
          var sells=JSON.parse(msg.asks);
          console.log(sells);

          if(buys.length>0){
           that.isSocketIn=false; 
          }else{
            that.isSocketIn=true;
          }
          if(sells.length>0){
           that.isSocketOut=false; 
          }else{
            that.isSocketOut=true;
          }
          that.outlist2=sells;
          that.inlist2=buys;
          // that.newData = msg.last_price;
          var priceData = {
             buyPrice:buys.length !=0?buys[0][0]:'',
             sellPrice:sells.length !=0?sells[sells.length-1][0]:''
          }
           eventBus.$emit("priceToTrade", priceData);
        }
      });
    }
  },


};
</script>

<style scoped>
.exchange{
  height: 100%;
}
.title {
  height: 48px;
  line-height: 48px;
  padding: 0 10px 0 30px;
  /* background-color: #181b2a; */
}
.content {
  padding: 0 10px;
}
.new_price {
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid #ccc;
  padding: 0 0 0 20px;
}
.exchange_title {
  line-height: 25px;
  position: relative;
  /* color: #637085; */
}
.list-item li {
  line-height: 25px;
  overflow: hidden;
}
.list-item li span,
.exchange_title span {
  width: 33.3%;
  display: inline-block;
  float: left;
}
.green {
  color: #008069;
}
.red {
  color: #cc4951;
}

.line {
  height: 5px;
  border-bottom: 1px solid #ccc;
}
/********全站交易*******/
.title{height: 48px;line-height: 40px;padding: 0 10px;}
.tab_title{display: inline-block;line-height: 30px;height: 30px;}
.tab_title span{cursor: pointer;}
.tab_title span:not(:last-child) {margin-right: 40px;}
.content{padding: 0 10px;height: 370px;}
.list-title{line-height: 40px; border-bottom: 1px solid #ccc;height: 40px;}
.list-title li{
    width: 25%;
    text-align: center;
}
.no_data{padding: 50px 0;}
.containers{height: 330px;overflow: auto;}
.list-item li{line-height: 30px; display: flex;}
.list-item li span{display: inline-block; float: left; width: 33.3%;text-align: center;}

/* .list-item li:hover{background-color: #eee; color: #de5959;} */
.list-item li span.green{color: #36a792}
.containers ul li{
    display: flex;
    justify-content: space-around;
}
</style>
