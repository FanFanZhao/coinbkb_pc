<template>
    <div id="pay-optsw">
      <p>{{$t('header.candychange')}}</p>
      <div class="tips  ">
       <div>
            <p>{{$t('account.notice')}}：</p>
           
       </div>
       <div class=" tc">
           <div>
                <a href="javascript:;" class="ft14 msg red" @click="openQQ">{{$t('header.candy_tip')}}:937068464</a>
           </div>
           
           <img src="@/assets/images/qq_qun.png" alt="" class="w30">
       </div>
      </div>
       
        <div class="inp-item tc">
            {{$t('header.candy_balance')}} {{balance}}
        </div>
        <div class="btn bgRed blue_bg" @click="add">
            {{$t('header.candy_duihuan')}}
        </div>
    </div>
</template>
<script>

export default {
    name:"",
    data (){
        return{
           balance:0,
        }
    },
    created(){
        var _this=this;
        _this.token=localStorage.getItem('token');
        _this.$http({
            url: "/api/candy/detail",
            method: "get",
            data: {
                
            },
            headers: { 'Authorization': _this.token,'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8', }
        }).then(res => {
            if(res.data.type=='ok'){
                 var message=res.data.message;
                _this.balance=(message.number-0)+(message.invite-0);
            }
        })
    },
    methods:{
        openQQ(){
            window.open('https://jq.qq.com/?_wv=1027&k=5MhfOq7');
        },
        add(){
            // layer.msg(this.$t('header.candy_notopen'));
            // return ;
            var _this=this;
            _this.$http({
                url: "/api/candy/exchange",
                method: "get",
                data: {
                    number:_this.balance
                },
                headers: { 'Authorization': _this.token,'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8', }
            }).then(res => {
                layer.msg(res.data.message)
            })
        }
    },
    mounted(){
      
    }
    
}
</script>
<style scoped lang='scss'>

#pay-optsw {
  width: 60%;
  margin: 50px auto;
  background: #fff;
  padding: 30px;
  > .inp-item {
    position: relative;
    margin-bottom: 20px;
    border: 1px solid #ddd;
    height: 42px;
    line-height: 42px;
    text-align: center;
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
  .btn {
    margin: 30px auto;
    width: 200px;
    line-height: 40px;
    text-align: center;
    background: #563BD1;
    color: #fff;
    cursor: pointer;
  }
  .tips{
    padding: 18px 15px;
    margin: 20px 0;
    border: 1px solid #eee;
    background: #F9FCFF;
  }
  .msg{
    line-height: 45px;
  }
}

</style>


