<template>
    <div class="entrust clr-part">
       <div class=" title ">
           我的糖果
       </div>
       <div class="w50 pdlr20">
           <p class="pdb20 flex alcenter between"><span>领取数量</span><span>{{number}}糖果</span></p>
           <p class="flex alcenter between"><span>邀请奖励</span><span>{{invite}}糖果</span></p>
       </div>
    </div>
</template>
<script>

export default {
    name:"",
    data (){
        return{
           number:0,
           invite:0
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
                    _this.number=res.data.number;
                    _this.invite=res.data.invite;
                }
            })
    },
    methods:{
    
    },
    mounted(){
      
    }
    
}
</script>
<style scoped>
.entrust{
    width: 80%;
    margin: 50px auto;
    background: #fff;
    padding: 30px;
}
.remove{
    color: #d45858!important;
}
.title{height: 48px;line-height: 46px;border-bottom: 1px solid #eee;margin-bottom: 30px;}
.mauto{
    margin: 0 auto;
}
.pdb20{padding-bottom: 20px;}

</style>


