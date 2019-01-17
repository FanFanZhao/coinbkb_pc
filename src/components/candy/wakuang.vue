<template>
    <div class="wk_box">
        <div class="content  ft12">
            <div class="content_top flex alcenter fColor2 bd">
               
                <p class="flex1 tc">{{$t('number')}}</p>
                <p class="flex1 tc">{{$t('legal.traderecord')}}</p>
                <p class="flex1 tc">{{$t('time')}}</p>
            </div>
            <ul class="content_ul">
                <li class="pdt10" v-for="(item,i) in list" :key="i">
                    <div class="content_li flex alcenter between bdr-part">
                        <p class="flex1 tc">{{item.change}}</p>
                        <p class="flex1 tc">{{item.memo}}</p>
                        <p class="flex1 tc light_blue">
                            {{item.create_time}}
                        </p>
                    </div>
                </li>
                <li class="no_rec mt10 light_blue tc" v-show="list.length !=0" @click="more">{{moreLog}}</li>
                <li class="no_rec mt10 light_blue tc" v-show="list.length ==0">{{$t('nodata')}}</li>
                
            </ul>
        </div>
    </div>
</template>
<script>
export default {
   data(){
       return {
           list:[],
           moreLog:this.$t('more'),
           page:1
       }
   },
   created(){
       this.getdata();
   },
   methods:{
       more(){
           var _this=this;
           _this.moreLog=this.$t('loading');
           _this.page++;
           _this.getdata();
       },
       getdata(){
            var _this=this;
            _this.token=localStorage.getItem('token');
            _this.$http({
                url: "/api/wallet/legal_log",
                method: "post",
                data: {
                    type:'ltc',
                    page:_this.page
                },
                headers: { 'Authorization': _this.token,'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8', }
            }).then(res => {
                console.log(res);
                if(res.data.type=='ok'){
                    var message=res.data.message;
                    _this.list=_this.list.concat(message.list);
                    if(message.list.length!=0){
                        _this.moreLog=_this.$t('more');
                    }else{
                        _this.moreLog=_this.$t('nomore');
                    }
                }
            })
       }
   } 


}
</script>
<style scoped>
    .wk_box{
        width: 60%;
        margin: 50px auto;
        background: #fff;
        padding: 30px;
    }
    .content_top{
        height: 45px;
        line-height: 45px;
        border-bottom: 1px solid #eee;
    }
    .pdt10{padding-top: 10px;}
</style>


