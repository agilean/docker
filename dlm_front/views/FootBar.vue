<template>
  <div class="page-tabbar">

    <div class="page-wrap">

      <div class="page-title">
          <mt-header fixed title="好吃点农场">
            <router-link to="/" slot="left">
            <mt-button icon="back">返回</mt-button>
           </router-link>
          <mt-button  slot="right">
              <mt-badge type="error">10</mt-badge>
           </mt-button>
       </mt-header>
       
      </div> <!-- end of page-title -->
      
      <div class="slider" v-show="adShow">
         <mt-swipe>
         <mt-swipe-item>
           好消息！农场有新货啦！
           <img src="../images/ban1.jpg">
         </mt-swipe-item>
         <mt-swipe-item>
          六月夏令营强势来袭！
          <img src="../images/ban2.jpg">
        </mt-swipe-item>
         <mt-swipe-item>
           热带作物新品引进花都！
           <img src="../images/ban3.jpg">
         </mt-swipe-item>
      </mt-swipe>
      
      </div> <!-- end of slider -->
      
      
      
        <mt-cell class="page-part" title="当前选中" :value="selected" />
      
      
      

      <mt-tab-container class="page-tabbar-container" v-model="selected">
        
    
               <mt-tab-container-item id="主页">
                
                  <div v-show="list.length">
               
                   <Product v-for="item in list" :info="item" :key="item.id">
                   </Product>
                   
                   <div class="product-not-found" v-show="!list.length">
                        暂无相关信息
                   </div>
               
                 </div>  
                 
                 <div> Popup here . </Popup> </div>
                 
                     
              </mt-tab-container-item>
        <mt-tab-container-item id="客服">
          
          
             <mt-cell title="默认" is-link>
             <span>广东省广州市天河区天润路124号</span>
             <img slot="icon" src="../images/kefu.png" width="60" height="60">
            </mt-cell>
             <mt-cell title="收件地址1" is-link>
             <span>广东省广州市天河区天润路124号</span>
            
            </mt-cell>
             <mt-cell title="收件地址2" is-link>
             <span>广东省广州市天河区天润路124号</span>
             </mt-cell>
          
        </mt-tab-container-item>
        <mt-tab-container-item id="门店">
          
          <Product/>
          
        </mt-tab-container-item>
        
        <mt-tab-container-item id="购物车">
          
            <Cart/> 
          
        </mt-tab-container-item>
        
        <mt-tab-container-item id="我的">
           
          <router-link to="/">
            <mt-button type="danger" size="large">退出</mt-button>
          </router-link>
        </mt-tab-container-item>
      </mt-tab-container>
      
    </div> <!-- end of page-wapper -->

    <mt-tabbar v-model="selected" fixed>
      <mt-tab-item v-for="menu in menus.icons" :id="menu.name" @click.native="switchIcon(menu.image)">
        
        <img slot="icon" :src="menu.image" style="cursor:pointer;" >  
            {{ menu.name }}  
         </img>
         
         
        
      </mt-tab-item>
    
    </mt-tabbar>
  </div> <!-- end of page-tabber -->
  
  
</template>

<script>
 
import Product from './product.vue';
import Cart from './cart.vue';
import Popup from './popup.vue';


export default {
  components:{Product,Cart},
  computed:{
     list () {
       return this.$store.state.productList;
     },
     
      menus (){
         return  this.$store.state.menus;
     },
     adShow () {
     
         return this.$store.state.adShow;
     }
     
  
  },
  methods:{
    
      switchIcon (src) {
         if(src.indexOf('Home') === -1 ){
           this.$store.dispatch('getAdShow',false);
         }
         this.$store.state.menus.icons.forEach(function(item){
              if(src == item['image'])//点击的ICON变成激活图片
              {
                
                if(src.indexOf('_active.png') === -1)//not active
                {
                   var len=item['image'].indexOf(".png");
                   var preffix=src.substring(0, len);
                   item['image']=preffix+'_active.png';
                }
              
              }else{//未点击的ICON全部变成未激活图片
                var len =item['image'].indexOf('_active.png');
                if(len > 0)
                {
                   var preffix=item['image'].substring(0, len);
                   item['image']=preffix+'.png';
                }
              }
              
             
         });
     }
  
  },
  mounted(){
      //mounted 阶段初始化商品列表, 存储在store.state.productList中。
      this.$store.dispatch('getProductList');
      this.$store.dispatch('getMenus');
      
      
  
  },
 
  name: 'FootBar',
  
  data() {
    return {
   
      selected: '主页'
     
      
    };
  },
  
  watch: {
        selected: function (val, oldVal) {
        //alert(val+','+oldVal);
        
      }
    }
};
</script>

<style scoped>
  .page-tabbar {
    overflow: hidden;
  }
  .slider{
       height:300px;
       padding:1px 3px;
       border-bottom:1px solid gray;
   }
   
 
   .page-wrap {
    overflow: auto;
    height: 100%;
    padding-top:40px;
    padding-bottom: 60px;
    
    
 
  }
  
  
</style>
