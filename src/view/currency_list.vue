<template>
    <div class="wrap currency-lst">
        <p class="tr apply">
            <router-link class=" red " to="/currencyApply" >上币申请</router-link>
        </p>
        <ul class="ul">
            <li class="flex alcenter list_title ft14">
                <span class="flex1 tc">币种名称</span>
                <span class="flex2 tc">投票进度</span>
                 <span class="flex1 tc">操作</span>
            </li>
            <li v-for="(item,index) in currency_list" :key="index" class="flex alcenter list_item ft12">
                <span class="flex1 tc">{{item.name}}</span>
                <div class="flex2 pro flex alcenter jscenter">
                    <div class=" tc pro_box">
                        <div :style="{width:item.count/500*100+'%'}"></div>
                    </div>
                    <p class="w20 flex between"><span class="">{{(item.count/500*100).toFixed(2)+'%'}}</span>  <span class="">{{item.count}}票</span></p>
                </div>
                
                 <span class="flex1 tc red" @click="edit(item.id)">投票</span>
            </li>
        </ul>
        <pre class="mt20 red" style="line-height:1.5">
            免费投票上币规则:
            <br>
            1；币种申请后开启上币投票，10天倒计时。
            2；币种投票满额500票视为投票成功，三天内上架币种开放交易，未完成500票视为上币失败。
            3；每一位用户单个币种只有一次投票机会，不能重复投票，投票需实名认证。
            4；每投一票消耗10枚bkbc。
            5；免费投票上币,三天内上线交易。
        </pre>
    </div>
</template>
<script>
export default {
     data(){
         return{
            currency_list:[],
            per:0,
         }
     },
     created(){
        // this.token = window.localStorage.getItem("token") || "";
        // if(this.token == ''){
        // this.$router.push('/components/login');
    // }
     },
     mounted(){
        this.init();
     },
     methods:{
         init(){
             this.$http({
                  url: "/api/submit/submit_list",
                  method: "get",

             }).then(res =>{
                  console.log(res);
                  if(res.data.type == 'ok'){
                      this.currency_list = res.data.message;
                  }
             })
         },
         edit(id){
             var _this=this;
               _this.token = window.localStorage.getItem("token") || "";
                if(_this.token == ''){
                    _this.$router.push('/components/login');
                    return;
                }
            
              _this.$http({
                  url: "/api/vote/add_vote",
                  data:{
                      id:id
                  },
                  method: "post",
                headers: { Authorization: localStorage.getItem("token") }
             }).then(res =>{
                  console.log(res);
                  layer.msg(res.data.message) ;

                  if(res.data.type=='ok'){
                     _this.init();
                  }
                 
             })
            
         }
     }
}
</script>
<style scoped>
    .wrap{
        width: 80%;
        margin: 50px auto;
        min-height: 700px;
    }
    .ul li{
        padding: 20px 0;
        border-bottom: 1px solid #eee;
    }
    .jscenter{
        justify-content: center;
    }
    .list_title{
        background: #eee;
    }
    .list_item:hover{
        background: #eee;
    }
    .red:hover{
        cursor: pointer;
    }
    .pro_box{
        background: #ddd;
        height: 16px;
        line-height: 16px;
        text-align: center;
        width: 78%;
        margin-right: 20px;

    }
    .pro_box >div{
        background: #d45858;
        height: 16px;
        color: #d45858;
    }
    .apply{
        padding-bottom:20px;
        font-size: 20px;
    }
</style>
