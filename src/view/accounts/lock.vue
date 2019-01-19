<template>
    <div class="bgf8 bg-part clr-part main-wrap">
        <div class="top">
            <p> {{$t('account.lock_balance')}}：{{total}} BKBC</P>
        </div>
        <div class="tc bdb_d pdtb15 plr15 ">
            <p class="top">{{$t('account.lock_number')}}：20000 {{$t('account.mei')}}</p>
            <div class="w15 lock mauto " @click="lock">{{$t('account.lock_confirm')}}</div>
        </div>
        <ul class="top lock_list">
            <li class="flex ft14">
                <div class="flex1 tc">{{$t('account.lock_number')}}</div>
                <div class="flex1 tc">{{$t('account.lock_remain')}}</div>
                <div class="flex1 tc">{{$t('c2c.timer')}}</div>
            </li>
            <li class="flex pdtb_10" v-if="lock_list&&lock_list.total" >
                <div class="flex1 tc">{{lock_list.total}}</div>
                <div class="flex1 tc">{{lock_list.number}}</div>
                <div class="flex1 tc">{{lock_list.time}}</div>
            </li>
            <li class="no_rec mt20 light_blue tc ft14" v-else >{{$t('nodata')}}</li>
        </ul>
    </div>
</template>
<script>
export default {
    data(){
        return{
          total:'',
          lock_list:{}
        }
    },
    created(){
       this.token = localStorage.getItem('token') || '';
    },
    mounted(){
       this.getbalance();
       this.getLocklist();
    },
    methods:{
         //获取余额
         getbalance(){
            
             var that = this
                 this.$http({
                    url: '/api/lock/balance',
                    method:'get',
                    data:{},
                    headers: {'Authorization':  that.token}
                }).then(res=>{
                        
                        if(res.data.type  =='ok'){
                            this.total = res.data.message;
                        }else{
                            layer.msg(res.message);
                        }
                    }).catch(error=>{
                        console.log(error)
                })
        },
        getLocklist(){
            var _this=this;
             var load = layer.load();
            _this.$http({
                url:'/api/lock/my_lock',
                method:'get',
                data:{number:20000},
                headers:{'Authorization':_this.token}
            }).then(res=>{
                layer.close(load);
                if(res.data.type=='ok'){
                    _this.lock_list=res.data.message;               
                }
            })
        },
        lock(){
            var _this=this;
             var load = layer.load();
            _this.$http({
                url:'/api/lock/lock',
                method:'post',
                data:{number:20000},
                headers:{'Authorization':_this.token}
            }).then(res=>{
                layer.close(load);
                layer.msg(res.data.message);
                if(res.data.type=='ok'){
                    _this.refresh()
                }
            })
        }
       
    }
}
</script>
<style scoped lang='scss'>
    .flex1{
        flex: 1;
    }
   .top{
       padding: 15px 30px;
   }
    .bdb_d{
        border-bottom: 1px solid #ddd;
    }
    .lock{
        background: #194B64 ;
        color: #fff;
        padding-top: 10px;
        padding-bottom: 10px;
        margin: 15px auto;
    }
     .main-wrap{
       height: 820px;
       overflow: auto;
   }
    .pdtb_10{
        padding-top: 10px;
        padding-bottom: 10px;
    }
</style>


