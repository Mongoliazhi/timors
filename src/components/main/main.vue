<template>
  <div class="main_1">
<!--    <transition name="play-slide"
                @after-leave="routerViewAnimation='page-slide'">
      <div class="userC">
        <h1>7878787878</h1>
      </div>
    </transition>-->


    <div class="index_header">
      <div class="index_line_fa">
        <p class="index_line"></p>
      </div>
      <div class="index_con">
        <div class="index_son_bg"></div>
        <div class="index_son" >
          <!--@click="userLogin"-->
          <div class="index_conImg" @click="userLogin">
            <template v-if="getUser.user_headImg">
              <img :src="getUser.user_headImg" alt="">
            </template>
            <template v-else>
              <img :src="user_headImg" alt="">
            </template>
          </div>
          <p class="index_conP">
            <em><span>Timor</span>狮</em>
            <i>Timors，整装待发</i>
          </p>
          <div class="index_conDiv">
            <a><input type="text" disabled="disabled" placeholder=""></a>
            <!--<i class="iconfont gj-search"></i>-->
            <svg class="icon" aria-hidden="true">
              <use xlink:href="#icon-jiantou122"></use>
            </svg>
          </div>
        </div>
      </div>
    </div>

    <div id="slider" class="mui-slider" style="height: 13rem">
      <div class="mui-slider-group mui-slider-loop" style="height: 13rem">
        <!-- 额外增加的一个节点(循环轮播：第一个节点是最后一张轮播) -->
        <div class="mui-slider-item mui-slider-item-duplicate">
          <a href="#">
            <img src="../../../static/images/yuantiao.jpg">
          </a>
        </div>
        <!-- 第一张 -->
        <div class="mui-slider-item">
          <a href="#">
            <img src="../../../static/images/shuijiao.jpg">
          </a>
        </div>
        <!-- 第二张 -->
        <div class="mui-slider-item">
          <a href="#">
            <img src="../../../static/images/muwu.jpg">
          </a>
        </div>
        <!-- 第三张 -->
        <div class="mui-slider-item">
          <a href="#">
            <img src="../../../static/images/cbd.jpg">
          </a>
        </div>
        <!-- 第四张 -->
        <div class="mui-slider-item">
          <a href="#">
            <img src="../../../static/images/yuantiao.jpg">
          </a>
        </div>
        <!-- 额外增加的一个节点(循环轮播：最后一个节点是第一张轮播) -->
        <div class="mui-slider-item mui-slider-item-duplicate">
          <a href="#">
            <img src="../../../static/images/shuijiao.jpg">
          </a>
        </div>
      </div>
      <div class="mui-slider-indicator">
        <div class="mui-indicator mui-active"></div>
        <div class="mui-indicator"></div>
        <div class="mui-indicator"></div>
        <div class="mui-indicator"></div>
      </div>
    </div>

    <div class="mui-content">
      <div id="Gallery" class="mui-slider">
        <div class="mui-slider-group">
          <div class="mui-slider-item">
            <ul class="mui-table-view mui-grid-view mui-grid-9">
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <router-link to="/travelList">
                      <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-buhang"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">旅行</div>
                </router-link>
              </li>
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <a href="#">
                          <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-jianshen"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">运动</div>
                </a>
              </li>
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <router-link to="/fundList">
                      <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-project"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">基金</div>
                </router-link>
              </li>
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <router-link to="/chatRoomList">
                          <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-kafei"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">聊天室</div>
                </router-link>
              </li>
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <a href="#">
                          <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-dianying"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">电影</div>
                </a>
              </li>
              <li class="mui-table-view-cell mui-media  mui-col-xs-4">
                <a href="#">
                         <span class="mui_dps">
                            <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-shoubing"></use>
                            </svg>
                      </span>
                  <div class="mui-media-body">游戏</div>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <template v-for="item in goodsList">
        <div @click="goodsDetail(item.goods_id)" class="mui-card">
          <div class="mui-card-header mui-card-media goodsList_img">
            <img :src="item.goods_picArr | getImgOne">
          </div>
          <div class="mui-card-content">
            <div class="mui-card-content-inner">
              <p>
                <span>{{item.goods_addTime | time}}</span>
                <span class="mui-pull-right">
                           <svg class="icon" aria-hidden="true">
                              <use xlink:href="#icon-zhu"></use>
                            </svg>
                      100</span>
              </p>
              <p>{{item.goods_name}}</p>
            </div>
          </div>
        </div>
      </template>

    </div>
    <v-footer></v-footer>


  </div>
</template>

<script>
  import * as _fetch from '../../config/fetch'
  import filter from '../../config/filter'
  import bus from '../../bus/bus'

  import Mock from 'mockjs'
  import footer from '../footer/footer.vue'
  import {mapState, mapGetters, mapActions, mapMutations} from 'vuex'

  export default {
    name: 'main_1',
    data() {
      return {
        goodsList: [],
        user_headImg: Mock.Random.image('500x500', '#8B87C1', '#ffffff', 'T'),
      }
    },
    computed: {
      ...mapState({
        userDate: state => state.User.user
      }),
      //mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：
      ...mapGetters([
        'getUser'
      ])
    },
    components: {
      'v-footer': footer
    },
    create:{
//      this.userDate.user_headImg = Mock.Random.image('500x500', '#8B87C1', '#ffffff', 'T');
    },
    methods: {

      menu() {
        var height = $(".index_header").height();
        this.scroll = $(window).scrollTop();
        if (this.scroll >= height) {
          $(".index_header").css("position", "fixed")
          $(".index_conImg").hide()
          $(".index_conP").hide()
          $(".index_conDiv a").css("width", "100%")
          $(".index_conDiv svg").css("top", "25%")
          $(".index_conDiv input").attr({
            "placeholder": "请输入关键字"
          })
          $(".index_con").css("padding", "0.5rem")
        } else {
          $(".index_header").css("position", "relative")
          $(".index_conImg").show()
          $(".index_conP").show()
          $(".index_conDiv svg").css("top", "34%")
          $(".index_conDiv input").attr({
            "placeholder": ""
          })
          $(".index_con").css("padding", "0.2rem 0.5rem")
        }
      },
      addEventListener: function () {
        window.addEventListener('scroll', this.menu);
      },
      ...mapMutations([
        'isLogin',
      ]),
      userLogin() {
        this.$router.push({path: '/login'})
      },
      //首页获取商品列表
      mainGetgoodslist() {
        var self = this;
        _fetch.fetch("/mainGetgoodslist", {
          pageIndex: 1,
          pageSize: 6
        }).then(($res) => {
          self.goodsList = $res.data
        }).catch((error) => {
          console.log(error)
        })
      },
      goodsDetail(goodsId) {
        this.$router.push({name: 'GoodsDetail', params: {goodsId: goodsId}})
      }
    },
    watch: {
      goodsList: function () {
        this.$nextTick(function () {
          $(".mui-card-media").height("6rem")
          $(".goodsList_img").height($(".mui-card-media").width())
          $(".goodsList_img").css({
            'padding': '0'
          })
          $(".goodsList_img img").css({
            'width': "100%",
            'height': '100%'
          })
        })
      },
//      userDate: function (val) {
//        console.log(val)
//        if (val.user_name) {
//          console.log("用户已登录")
//        } else {
//          console.log("用户未登录")
//          this.userDate.user_headImg = Mock.Random.image('500x500', '#8B87C1', '#ffffff', 'T');
//        }
//      }
    },
    mounted() {
      var self = this;
      this.$nextTick(function () {
//          监听滚动
        console.log(JSON.parse(sessionStorage.getItem("user") || '{}'))
        console.log("1-1-1-1-1")
        console.log(this.userDate)
        console.log(this.getUser)

        self.addEventListener(); //滚动监听
        self.mainGetgoodslist(); //获取商品列表
//      切换导航栏背景颜色
        setInterval(function () {
          $(".index_son_bg").css("background-color", Mock.mock('@color'))
        }, 3000)

        mui.init({
          swipeBack: false //启用右滑关闭功能
        });
        var slider = mui("#slider");
        slider.slider({
          interval: 3000
        });

      })
    },


  }
</script>

<style scoped>

  .main{
    /*height: 640px;*/
  }
  .index_line_fa {
    position: relative;
    width: 100%;
    overflow: hidden;
    height: 2px;
  }

  .index_line {
    position: absolute;
    left: 0%;
    top: 0%;
    width: 200%;
    background: linear-gradient(to right, #DF9E49, #EDD87B, #E8B659, #DA9D42, #FCECCB, #FFFFFD, #EDCF81, #E6BD6B, #C77625);
    height: 100%;
    animation: mymove 5s infinite linear;
    -webkit-animation: mymove 5s infinite linear;
  }

  @keyframes mymove {
    0% {
      left: 0%;
    }
    100% {
      left: -100%;
    }
  }

  .index_header {
    position: relative;
    width: 100%;
    top: 0;
    left: 0;
    z-index: 100;
  }

  .index_header .index_con {
    width: 100%;
    overflow: auto;
    padding: 0.2rem 0.5rem;
  }

  .index_header .index_son_bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: mediumvioletred;
    opacity: 0.7;
    z-index: 101;
  }

  .index_son {
    position: relative;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    flex-direction: row;
    -moz-justify-content: space-between;
    -webkit-justify-content: space-between;
    justify-content: space-between;
    z-index: 102;
  }

  .index_son .index_conImg {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    -webkit-flex-direction: column;
    flex-direction: column;
    -webkit-box-align: center;
    -moz-align-items: center;
    -webkit-align-items: center;
    align-items: center;
    -moz-justify-content: center;
    -webkit-justify-content: center;
    justify-content: center;
  }

  .index_son .index_conImg img {
    width: 100%;
    border-radius: 50%;
  }

  .index_son .index_conP {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-flex: 2;
    -webkit-flex: 2;
    flex: 2;
    -webkit-flex-direction: column;
    flex-direction: column;
    -webkit-box-align: center;
    -moz-align-items: center;
    -webkit-align-items: center;
    align-items: center;
    -moz-justify-content: center;
    -webkit-justify-content: center;
    justify-content: center;
    padding-left: 3%;
  }

  .index_son .index_conP em {
    display: block;
    width: 100%;
    color: #fff;
    font-size: 16px;
    text-align: left;
  }

  .index_son .index_conP em span {
    color: #01CFCF;
  }

  .index_son .index_conP i {
    display: block;
    width: 100%;
    font-size: 12px;
    color: #fff;
  }

  .index_son .index_conDiv {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    position: relative;
    -webkit-box-flex: 3;
    -webkit-flex: 3;
    flex: 3;
    -webkit-flex-direction: column;
    flex-direction: column;
    /*-webkit-box-align: center;*/
    /*-moz-align-items: center;*/
    /*-webkit-align-items: center;*/
    /*align-items: center;*/
    -moz-justify-content: center;
    -webkit-justify-content: center;
    justify-content: center;
  }

  .index_son .index_conDiv input {
    margin-bottom: 0;
    height: 2rem;
  }

  .index_son .index_conDiv i {
    position: absolute;
    right: 5px;
    top: 34%;
  }
  .index_son .index_conDiv svg {
    position: absolute;
    right: 5px;
    top: 34%;
  }

  .mui_dps {
    font-size: 2.5rem;
  }

  .mui-grid-view.mui-grid-9 .mui-table-view-cell {
    padding: 0 10px;
  }

  .mui-slider .mui-slider-group .mui-slider-item {
    width: 100%;
    height: 100%;
  }
  .mui-slider .mui-slider-group .mui-slider-item a{
    width: 100%;
    height: 100%;
  }
  .mui-slider .mui-slider-group .mui-slider-item a img{
    width: 100%;
    height: 100%;
  }
</style>

