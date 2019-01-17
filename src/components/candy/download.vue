<template>
     <div id="pay-opts">
      <p>{{$t('footer.download')}}</p>
        <div class="btn bgRed blue_bg flex mt40 pdlr20 pdtb20 jscenter">
            <div class="item positionR flex alcenter  jscenter mr20 ">
                <img src="@/assets/images/an.png" alt="" class="img_logo mr10">
                <span>{{$t('footer.anzhuo')}}</span>
                <img src="@/assets/images/an_qr.png" alt="" class="mt20 abst ewm">
            </div>
            <div class="item positionR flex alcenter  jscenter mr20 ">
                <img src="@/assets/images/an.png" alt="" class="img_logo mr10">
                <span>{{$t('footer.appledown')}}</span>
                <img src="@/assets/images/an_qr.png" alt="" class="mt20 abst ewm">
            </div>
        </div>
    </div>
</template>
<script>
export default {
    
}
</script>
<style scoped lang='scss'>
#pay-opts {
  width: 60%;
  margin: 50px auto;
  background: #fff;
  padding: 50px;
  .item{
    width: 180px;
    height: 50px;
    border-radius: 25px;
    line-height: 50px;
    border: 1px solid #c9c8f1;
  }
  .img_logo{
    width: 26px;
  }
  .ewm{
    //   display: none;
      top: 40px;
      left: 50%;
      width: 150px;
      height: 150px;
      transform: translateX(-50%);
    }
    .blue_bg{
        padding-bottom: 200px;
    }
}
.mr10{
    margin-right: 10px;
}
.jscenter {
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center; 
}
.abst{
    position: absolute;
}
</style>


