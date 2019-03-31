<template>

    <div class="wrap">
        <indexHeader></indexHeader>
        <div class="box">
        <iframe src="../../static/Coinbkb.pdf" frameborder="0"></iframe>
        </div>
    </div>
</template>
<script>
import indexHeader from '@/view/indexHeader'
export default {
    components:{
        indexHeader
    }
    
}
</script>
<style scoped>
   .box{
       width: 80%;
       margin: 20px auto;
       height: 700px;
   }
   .box iframe{
       width: 100%;
       height: 100%;
   }
</style>
