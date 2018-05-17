<template>
  <div class="header">
    <div class="content-wrapper">
         <div class="avatar">
           <img  :src="seller.avatar" alt="">
         </div>
         <div class="content">
            <div class="title">
              <span class="brand"></span><span class="name">{{seller.name}}</span>
            </div>
            <div class="description">
              {{seller.description}}/{{seller.deliveryTime}}分钟到达
            </div>
            <div v-if="seller.supports" class="support">
              <span class="icon" :class="classMap[seller.supports[0].type]"></span>
              <span class="text">{{ seller.supports[0].description }}</span>
            </div>
         </div>
         <div v-if="seller.supports" class="support-content" @click="showDetail">
                <span class="count">{{ seller.supports.length }}个</span>
                <i class="icon-keyboard_arrow_right"></i>
          </div>
    </div>  
    <div class="bulletin-wrapper" @click="showDetail">
        <span class="bulletin-title"></span><span class="bulletin-text">{{ seller.bulletin }}</span>
        <i class="icon-keyboard_arrow_right"></i>
    </div> 
    <div class="background">
        <img :src="seller.avatar" width="100%" height="100%">
    </div> 
    <transition name="fade">
        <div v-show="detailShow" class="detail">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{ seller.name }}</h1>
                    <div class="star-wrapper">
                        <star :size="48" :score="seller.score"></star>
                    </div>
                    <!--flex 布局-->
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class="supports">
                        <li class="support-item" v-for="(item, index) in seller.supports">
                            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                            <span class="text">{{ seller.supports[index].description }}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="bulletin">
                        <p class="content">{{ seller.bulletin }}</p>
                    </div>
                </div>
            </div>
            <div class="detail-close" @click="hideDetail">
                <i class="icon-close">X</i>
            </div>
        </div>
    </transition>                
  </div>
</template>
<script>
    import star from '../star/star';
    export default {
      props: {
        seller: {
          type: Object
        }
      },
      data(){
        return {
          detailShow:false
        }
      },
      created() {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      },
      methods: {
        showDetail() {
          this.detailShow = true;
        },
        hideDetail() {
            this.detailShow = false;
        }
      },
      components:{
        star
      }
    };
</script>

<style  lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin';

.header {
  position: relative;
  overflow: hidden;
  color: #fff;
  background: rgba(7, 17, 27, 0.5);

  .content-wrapper {
    position: relative;
    padding: 2.4rem 1.2rem 1.8rem 2.4rem;
    font-size: 0;

    .avatar {
      display: inline-block;
      vertical-align: top;

      img {
        border-radius: 0.2rem;
        width: 6.4rem;
        height: 6.4rem;
      }
    }

    .content {
      display: inline-block;
      margin-left: 1.6rem;

      .title {
        margin: 0.2rem 0 0.8rem 0;

        .brand {
          display: inline-block;
          vertical-align: top;
          width: 3rem;
          height: 1.8rem;
          bg-image('brand');
          background-size: 3rem 1.8rem;
          background-repeat: no-repeat;
        }

        .name {
          margin-left: 0.6rem;
          font-size: 1.6rem;
          line-height: 1.8rem;
          font-weight: bold;
        }
      }

      .description {
        margin-bottom: 1rem;
        line-height: 1.2rem;
        font-size: 1.2rem;
      }

      .support {
        .icon {
          display: inline-block;
          vertical-align: top;
          width: 1.2rem;
          height: 1.2rem;
          margin-right: 0.4rem;
          background-size: 1.2rem 1.2rem;
          background-repeat: no-repeat;

          &.decrease {
           bg-image('decrease_1');          }

          &.discount {
           bg-image('discount_1');          }

          &.guarantee {
           bg-image('guarantee_1');          }

          &.invoice {
            bg-image('invoice_1');          }

          &.special {
           bg-image('special_1');          }
        }

        .text {
          line-height: 1.2rem;
          font-size: 1rem;
        }
      }
    }
    .support-content {
        position: absolute;
        right: 1.2rem;
        bottom: 1.4rem;
        padding: 0 .8rem;
        height: 2.4rem;
        line-height: 2.4rem;
        border-radius: 1.4rem;
        background: rgba(0, 0, 0, 0.2);
        text-align: center;

        .count {
          vertical-align: top;
          font-size: 1.0rem;
        }

        .icon-keyboard_arrow_right {
          margin-left: .2rem;
          line-height: 2.4rem;
          font-size: 1.0rem;
        }
      }
      
  }
  .bulletin-wrapper {
      position: relative;
      height: 2.8rem;
      line-height: 2.8rem;
      padding: 0 2.2rem 0 1.2rem;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      background: rgba(7, 17, 27, .2);
      .bulletin-title {
          display: inline-block;
          vertical-align: top;
          margin-top: .8rem;
          width: 2.2rem;
          height: 1.2rem;
          bg-image('bulletin');
          background-size: 2.2rem 1.2rem;
          background-repeat: no-repeat;
      }
      .bulletin-text {
          vertical-align: top;
          margin: 0 .4rem;
          font-size: 1.0rem;
      }
      .icon-keyboard_arrow_right {
          position: absolute;
          font-size: 1.0rem;
          right: 1.2rem;
          top: .8rem;
      }
  }
  .background{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index -1;
    filter:blur(1rem)
  }
    .detail {
            position: fixed;
            top: 0;
            left: 0;
            z-index: 100;
            width: 100%;
            height: 100%;
            overflow: auto;
            transition: all .5s;
            background: rgba(7, 17, 27, .8);
            backdrop-filter: blur(1rem); /*ios 模糊背景*/
            &.fade-enter-active, &.fade-leave-active {
                transition: opacity .5s
            }
            &.fade-enter, &.fade-leave-active {
                opacity: 0
            }
            .detail-wrapper {
                width: 100%;
                min-height: 100%;
                .detail-main {
                    margin-top: 6.4rem;
                    padding-bottom: 6.4rem; /*sticky footer 必须有  高度为需要定位元素所占的高度*/
                    .name {
                        line-height: 1.6rem;
                        text-align: center;
                        font-size: 1.6rem;
                        font-weight: 700;
                    }
                    .star-wrapper {
                        margin-top: 1.8rem;
                        padding: .2rem 0;
                        text-align: center;
                    }
                    .title {
                        display: flex;
                        width: 80%;
                        margin: 2.8rem auto 2.4rem;
                        .line {
                            flex: 1;
                            position: relative;
                            top: -0.6rem;
                            border-bottom: 1rem solid rgba(255, 255, 255, .2);
                        }
                        .text {
                            padding: 0 1.2rem;
                            font-weight: 700;
                            font-size: 1.4rem;
                        }
                    }
                    .supports {
                        width: 80%;
                        margin: 0 auto;
                        .support-item {
                            padding: 0 1.2rem;
                            margin-bottom: 1.2rem;
                            font-size: 0;
                            &:last-child {
                                margin-bottom: 0;
                            }
                            .icon {
                                display: inline-block;
                                width: 1.6rem;
                                height: 1.6rem;
                                vertical-align: top;
                                margin-right: .6rem;
                                background-size: 1.6rem 1.6rem;
                                background-repeat: no-repeat;
                                &.decrease {
                                   bg-image('decrease_2');
                                }
                                &.discount {
                                     bg-image('discount_2');
                                }
                                &.guarantee {
                                    bg-image('guarantee_2');
                                }
                                &.invoice {
                                    bg-image('invoice_2');
                                }
                                &.special {
                                    bg-image('special_2');
                                }
                            }
                            .text {
                                font-size: 1.2rem;
                                line-height: 1.6rem;
                            }
                        }
                    }
                    .bulletin {
                        width: 80%;
                        margin: 0 auto;
                        .content{
                            padding: 0 1.2rem;
                            line-height: 2.4rem;
                            font-size: 1.2rem;
                        }
                    }
                }
            }
            .detail-close {
                position: relative;
                width: 3.2rem;
                height: 3.2rem;
                margin: -6.4rem auto 0; /*关键*/
                clear: both; /*关键*/
                font-size: 3.2rem;
            }
        }
}
</style>
