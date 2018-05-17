<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
          <li v-for="(item, index) in goods" :key="index"  class="menu-item" :class="{'current': currentIndex === index}"
              @click="selectMenu(index, $event)" ref="menuList"> 
              <span class="text border-1px">
                  <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{ item.name }}
              </span>
          </li>
      </ul>
    </div>
     <div class="foods-wrapper" ref="foodWrapper">
       <ul>
         <li v-for="item in goods"    class="food-list food-list-hook" ref="foodList">
              <h1 class="title">{{ item.name }}</h1>
              <ul>
                  <li v-for="(food,index) in item.foods" :key="index" class="food-item border-1px"
                      @click="selectFood(food, $event)">
                      <div class="icon">
                          <img :src="food.icon" style="width:5.7rem height=5.7rem">
                      </div>
                      <div class="content">
                          <h2 class="name">{{ food.name }}</h2>
                          <p class="desc">{{ food.description }}</p>
                          <div class="extra">
                              <span class="count">月售{{ food.sellCount }}份</span><span>好评率{{ food.rating }}%</span>
                          </div>
                          <div class="price">
                              <span class="now">￥{{ food.price }}</span><span v-show="food.oldPrice" class="old">￥{{ food.oldPrice }}</span>
                          </div>
                          <!-- <div class="cartcontrol-wrapper">
                            <cartcontrol @add="addFood" :food="food"></cartcontrol>
                        </div> -->
                      </div>
                  </li>
              </ul>
          </li>
       </ul>
    </div>
    <!-- <food :food="selectedFood" ref="food"></food> -->
  </div>
</template>

<script>
    import BScroll from 'better-scroll';
    export default{
        props:{
            seller:{
              type:Object
            }
        },
        data(){
            return {
            goods: [],
            listHeight: [],
            scrollY: 0,
            selectedFood: {}
            }
        },
        computed:{
            currentIndex() {
                for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                    this._followScroll(i);
                    return i;
                }
                }
                return 0;
            },
            selectFoods() {
                let foods = [];
                this.goods.forEach((good) => {
                    good.foods.forEach((food) => {
                        if (food.count) {
                            foods.push(food);
                        }
                    });
                });
                return foods;
            }
        },
        created(){
            const url='https://easy-mock.com/mock/5af2c90e98fb450b11cb0ffd/webjia/goods';
            this.$http.get(url).then((data)=>{
                console.log(data.body.goods)
                this.goods=data.body.goods;
                this.$nextTick(() => {
                    this._initScroll();
                    this._calculateHeight();
                });
            })
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        
        },
        methods:{
            selectMenu(index, event) {
                if (!event._constructed) {
                return;
                }
                let foodList = this.$refs.foodList;
                let el = foodList[index];
                console.log(el)
                this.foodScroll.scrollToElement(el, 300);
            },
            _initScroll() {
                this.menuScroll = new BScroll(this.$refs.menuWrapper, {
                    click: true
                });
                this.foodScroll = new BScroll(this.$refs.foodWrapper, {
                    click: true,
                    probeType: 3 // 监测实时滚动的位置
                });

                this.foodScroll.on('scroll', (pos) => {
                    console.log(pos);
                    // 判断滑动方向，避免下拉时分类高亮错误（如第一分类商品数量为1时，下拉使得第二分类高亮）
                    if (pos.y <= 0) {
                        this.scrollY = Math.abs(Math.round(pos.y));
                    }
                })
            },
            _calculateHeight() {
                // 获取食物的li Dom节点列表
                let foodList = this.$refs.foodList;
                console.log(foodList)
                let height = 0;
                this.listHeight.push(height);
                for (let i = 0; i < foodList.length; i++) {
                    let item = foodList[i];
                    height += item.clientHeight;
                    this.listHeight.push(height);
                }
                console.log(this.listHeight)
                console.log(height)
            },
            _followScroll(index) {
                let menuList = this.$refs.menuList;
                let el = menuList[index];
                this.menuScroll.scrollToElement(el, 300, 0, -100);
            }
        },
        components: {
            // food
        }
  }
</script>

<style lang="stylus" type="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods{
    display: flex;
    position:absolute
    top:17.4rem;
    bottom:4,6rem;
    width:100%;
    overflow hidden;
    .menu-wrapper{
      width:8rem;
      flex: 0 0 8rem;
      background :#f3f5f7;
      .menu-item{
        display :table;
        height:5.4rem;
        width:5.6rem;
        line-height 1.4rem;
        padding:0 1.2rem;
        &.current{
          position :relative;
          margin-top:-0.1rem;
          z-index :10;
          background #fff;
          font-weight 700;
          .text{
            @include border-none()
          }
        }
        .icon{
          display :inline-block;
          vertical-align :top;
          width:1.2rem;
          height:1.2rem;
          margin-right:.2rem;
          background-size :1.2rem 1.2rem;
          background-repeat :no-repeat;
          &.decrease {
              bg-image('decrease_3');
          }
          &.discount {
              bg-image('discount_3');
          }
          &.guarantee {
              bg-image('guarantee_3');
          }
          &.invoice {
            bg-image('invoice_3');
          }
          &.special {
              bg-image('special_3');
          }
        }
        .text{
          display :table-cell;
          width:5.6rem;
          vertical-align :middle;
          font-size 1.2rem;
          border-1px(rgba(7, 17, 27, .1));
        }
      }
    } 
    .foods-wrapper {
            flex: 1;
            .title {
                padding-left: 1.4rem;
                height: 2.6rem;
                line-height: 2.6rem;
                border-left: .2rem solid #d9dde1;
                font-size: 1.2rem;
                color: rgb(147, 153, 159);
                background: #f3f5f7;
            }
            .food-item {
                display: flex;
                margin: 1.8rem;
                padding-bottom: 1.8rem;
                border-1px(rgba(7, 17, 27, .1));
                &:last-child {
                    border-none();
                    margin-bottom: 0;
                }
                .icon {
                    flex: 0 0 5.7rem;
                    margin-right: 1.0rem;
                }
                .content {
                    flex: 1;
                    .name {
                        margin: .2rem 0 .8rem;
                        line-height: 1.4rem;
                        font-size: 1.4rem;
                        color: rgb(7, 17, 27);
                    }
                    .desc,
                    .extra {
                        font-size: 1.0rem;
                        color: rgb(147, 153, 159);
                    }
                    .desc {
                        line-height: 1.2rem;
                        margin-bottom: .8rem;
                    }
                    .extra {
                        line-height: 1.0rem;
                        .count {
                            margin-right: 1.2rem;
                        }
                    }
                    .price {
                        font-weight: 700;
                        line-height: 2.4rem;
                        .now {
                            margin-right: .8rem;
                            font-size: 1.4rem;
                            color: rgb(240, 20, 20);
                        }
                        .old {
                            text-decoration: line-through;
                            font-size: 1.0rem;
                            color: rgb(147, 153, 159);
                        }
                    }
                    .cartcontrol-wrapper {
                        position: absolute;
                        right: 0;
                        bottom: 1.2rem;
                    }
                }
            }
        }
  }
</style>

