<template>
    <div class="entrust clr-part">
       <div class="title">
           {{$t('header.mycandy')}}
       </div>
       <div class=" pdlr20">
           <p class="pdb20 flex alcenter between"><span>{{$t('header.candy_number')}}</span><span>{{number}}{{$t('header.candyname')}}</span></p>
           <p class="flex alcenter between"><span>{{$t('header.candy_invite')}}</span><span>{{invite}}{{$t('header.candyname')}}</span></p>
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
          _this.token=localStorage.getItem('token');
            _this.$http({
                url: "/api/candy/detail",
                method: "get",
                data: {
                },
                headers: { 'Authorization': _this.token,'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8', }
            }).then(res => {
                console.log(res);
                if(res.data.type=='ok'){
                    var message=res.data.message;
                    _this.number=message.number;
                    _this.invite=message.invite;
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
    width: 60%;
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


