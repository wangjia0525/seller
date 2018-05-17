<template>
  <div id="app">
      <v-header :seller="seller"></v-header>
    
      <div class="tab">
          <div class="tab-item">
            <router-link to="/goods">
              <a>商品</a>
            </router-link>
          </div>
          <div class="tab-item">
            <router-link  to="/ratings">
              <a>评论</a>
            </router-link>
          </div>
          <div class="tab-item">
            <router-link  to="/seller">
              <a>商家</a>
            </router-link>
          </div>
      </div>
      <router-view></router-view>
  </div>
</template>

<script>
  import header from "./components/header/header.vue";
  import {urlParse} from './common/js/util';
  export default{
    data(){
      return {
        seller:{
           id: (() => {
                let queryParam = urlParse();
                console.log(urlParse())
                return queryParam.id;
            })()
        }
      }
    },
    created(){
      this.$http.get('https://easy-mock.com/mock/5af2c90e98fb450b11cb0ffd/webjia/seller').then((data)=>{
        console.log(data);
         // 相当于 extend方法 扩展  es6语法   vue推荐的给对象扩展属性方法
        this.seller = Object.assign({}, this.seller,data.body.seller);
      });
      // this.$http.get('https://easy-mock.com/mock/5af2c90e98fb450b11cb0ffd/webjia/goods').then((data)=>{
      //   console.log(data);
        
      // })
      // this.$http.get('/api/goods').then((data)=>{
      //   console.log(data)
      // })
    },
    components:{
        "v-header":header
      }
    }
</script>


<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  .tab
    display: flex
    width: 100%
    height: 4rem
    line-height: 4rem
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 1.4rem
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>