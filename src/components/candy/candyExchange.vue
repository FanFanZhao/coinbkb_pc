<template>
    <div id="pay-opts">
      <p>糖果兑换</p>
      <div class="tips">
        <p>{{$t('account.notice')}}：</p>
        <a href="javascript:;" class="ft14 msg red" @click="openQQ">
            糖果兑换需进QQ群:937068464
		</a>
      </div>
       
        <div class="inp-item tc">
            可用糖果数量 {{balance}}
        </div>
        <div class="btn bgRed blue_bg" @click="add">
            兑换
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
        _this.$http({
            url: "/api/candy/detail",
            method: "get",
            data: {
                
            },
            headers: { 'Authorization': _this.token,'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8', }
        }).then(res => {
            if(res.data.type=='ok'){
                _this.balance=(res.data.number-0)+(res.data.invite-0);
            }
        })
    },
    methods:{
        openQQ(){
            window.open('tencent://message/?uin=937068464');
        },
        add(){
            var _this=this;
            _this.$http({
                url: "/api/candy/exchange",
                method: "post",
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

#pay-opts {
  width: 80%;
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


