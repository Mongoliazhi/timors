<template>
  <div class="chatRoom">
    <router-view></router-view>

    <v-nav :nav-name="'机器人'" :nav-num="0"></v-nav>
    <div class="cr_content">
      <div class="crCont crCont_left">
        <div class="crCont_contC">
          <p>星期三 下午09:00</p>
        </div>
      </div>
      <div class="crCont crCont_left">
        <div class="crCont_contC">
          <p>小发发</p>
        </div>
      </div>

      <template v-for="v in dataList.message">
        <template v-if="v.messageType == 'you'">
          <div class="crCont crCont_left">
            <div class="crCont_img"><img :src="v.user_face" alt=""></div>
            <div class="crCont_cont">
              <p class="p1">{{v.messageValue}}</p>
            </div>
          </div>
        </template>
        <template v-else>
          <div class="crCont crCont_right">
            <div class="crCont_img"><img :src="v.user_face" alt=""></div>
            <div class="crCont_cont1">
              <p class="p1">{{v.messageValue}}</p>
            </div>
          </div>
        </template>
      </template>


    </div>

    <div class="charRoom_foot">
      <div class="cr_fa">
        <div class="cr_div1">
          <img src="../../../static/images/shuohua.png" alt="">
        </div>
        <div class="cr_div2">
          <input v-model="writeMessage" @keydown.enter.prevent="sendMessage" type="text">
        </div>
        <div class="cr_div1">
          <img src="../../../static/images/biaoqing.png" alt="">
        </div>
        <div class="cr_div1" @click="sendMessage">
          <img src="../../../static/images/add1.png" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script>

  import {mapState, mapGetters, mapActions} from 'vuex'
  import nav from '../header/nav.vue'

  export default {
    name: 'chatRoom',
    data() {
      return {
        writeMessage:'',
        dataList:{
          user_id:'999',   //别人的id
          chatWith:'正在加载中...',  //自己对别人的备注
          status:'4G在线', //别人的设备状态
          user_face:'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2112873150,678007007&fm=27&gp=0.jpg',  //别人的头像
          beizhu:'机器人',  //别人对自己的备注
          message:[{
            messageType: 'you',
            messageValue:'你好！认识你很高兴。',
            user_face:'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2112873150,678007007&fm=27&gp=0.jpg',
            time:Date.parse(new Date())/1000  //时间
          },{
            messageType: 'you',
            messageValue:'请问你怎么称呼？',
            user_face:'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2112873150,678007007&fm=27&gp=0.jpg',
            time:Date.parse(new Date())/1000  //时间
          },{
            messageType:'me',
            messageValue:'叫我小发发就可以了',
            user_face:'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1913964804,3663601362&fm=27&gp=0.jpg',
            time:Date.parse(new Date())/1000  //时间
          },
          ]  //消息
        },
      }
    },
    computed: {
      ...mapGetters([
        'user',
      ]),
      ...mapState({
        userDate: state => state.User.user,
      })
    },
    components: {
      'v-nav': nav
    },
    watch: {},
    methods: {
      async sendMessage() {
        console.log(this.writeMessage.trim())
        if (this.writeMessage.trim() == '') return

        this.sendBySocket()  //先通过socket发送消息
        //本地追加自己发送的消息
        const data = {
          messageType:'me',
          messageValue: this.writeMessage,
          user_face:'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1913964804,3663601362&fm=27&gp=0.jpg',
          time:Date.parse(new Date())/1000  //时间
        }
        this.addMessageLocal(data)
        this.writeMessage=''
      },
      sendBySocket(){
        this.$socket.emit('oneMessage',{
          from_user: "111",  //发送方id（即自己的id）
          from_user_face:"https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1913964804,3663601362&fm=27&gp=0.jpg",//发送方头像（即自己的头像）
          from_user_beizhu:this.dataList.beizhu,//发送方名字（即别人对自己的备注）
          to_user:this.dataList.user_id,//接收方id
          message:this.writeMessage,//消息内容
          time:Date.parse(new Date())/1000 //时间
        })
      },
      addMessageLocal(data){
        console.log(this.dataList.message)
        this.dataList.message.push(data)
      },
      updateBySocket(){
        console.log("监听到oneReturnMessage1")
//        this.$socket.removeAllListeners()  //一定要先移除原来的事件，否则会有重复的监听器
        this.$socket.on('oneReturnMessage', (data) => {
          console.log("监听到oneReturnMessage2")
          console.log(data)
          this.dataList.message.push(data)
        });
      }
    },
    mounted() {
      var self = this;
      this.updateBySocket() //监听接受到的消息

      this.$nextTick(function () {

//        this.$socket.emit('message', "1213123123111");
//        this.$socket.on('news', function ($res) {
//          console.log("0-90-0-0")
//          console.log($res)
//        });
      })
    },
  }
</script>

<style scoped>
  .chatRoom {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 1000;
    background-color: #fff;
    margin-top: 3rem;
    overflow: auto;
  }

  /*聊天房间*/

  .cr_content {
    width: 100%;
    margin-bottom: 3rem;
    padding-bottom: 3rem;
  }

  .crCont {
    width: 100%;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    /*-webkit-flex-direction: row;*/
    /*flex-direction: row;*/
    padding: 0.5rem;
  }

  .crCont_img {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .crCont_img img {
    width: 100%;
  }

  .crCont_cont {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 7;
    -webkit-flex: 7;
    flex: 7;
    -webkit-flex-direction: row;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    padding-left: 1rem;
  }

  .crCont_cont .p1 {
    position: relative;
    background-color: #f151a8;
    color: #fff;
    padding: 0.5rem;
    border-radius: 5px;
  }

  .crCont_cont .p1::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    left: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent #f151a8 transparent transparent;
  }

  .crCont_cont .img1 {
    position: relative;
    border-radius: 0.5rem;
    text-align: left;
  }

  .crCont_cont .img1::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    left: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent #000 transparent transparent;
  }

  .crCont_cont .img1 img {
    width: 40%;
  }

  .crCont_cont .voice {
    position: absolute;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    padding-right: 1rem;
  }

  .crCont_cont .img2 {
    position: relative;
    width: 2rem;
    height: 2rem;
    border-radius: 5px;
    background-color: #fff;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
  }

  .crCont_cont .img2::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    left: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent #fff transparent transparent;
  }

  .crCont_cont .img2 img {
    width: 60%;
  }

  .crCont_cont .voice .voice_s {
    margin-left: 0.5rem;
  }

  .crCont_cont1 {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 7;
    -webkit-flex: 7;
    flex: 7;
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
    align-items: center;
    justify-content: flex-start;
    padding-right: 1rem;
  }

  .crCont_cont1 .p1 {
    position: relative;
    background-color: #1D81CF;
    color: #fff;
    padding: 0.5rem;
    border-radius: 5px;
  }

  .crCont_cont1 .p1::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    right: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent transparent transparent #1D81CF;
  }

  .crCont_cont1 .img1 {
    position: relative;
    border-radius: 0.5rem;
    text-align: right;
  }

  .crCont_cont1 .img1::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    right: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent transparent transparent #000;
  }

  .crCont_cont1 .img1 img {
    width: 40%;
  }

  .crCont_cont1 .voice {
    position: absolute;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
    align-items: center;
    justify-content: flex-start;
  }

  .crCont_cont1 .img2 {
    position: relative;
    width: 2rem;
    height: 2rem;
    border-radius: 5px;
    background-color: #1D81CF;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
  }

  .crCont_cont1 .img2::before {
    position: absolute;
    top: 50%;
    margin-top: -0.4rem;
    right: -0.7rem;
    content: '';
    border-width: 0.4rem 0.4rem 0.4rem 0.4rem;
    border-style: solid;
    border-color: transparent transparent transparent #1D81CF;
  }

  .crCont_cont1 .img2 img {
    width: 60%;
  }

  .crCont_cont .voice .voice_s {
    margin-right: 0.5rem;
  }

  .crCont_contC {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    -webkit-flex-direction: row;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }

  .crCont_contC p {
    background-color: #ABACB0;
    color: #fff;
    padding: 0.5rem;
    border-radius: 5px;
  }

  .crCont_left {
    -webkit-flex-direction: row;
    flex-direction: row;
  }

  .crCont_right {
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
  }

  .charRoom_foot {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    background-color: #fff;
  }

  .cr_fa {
    width: 100%;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-direction: row;
    flex-direction: row;
  }

  .cr_div1 {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    align-items: center;
    justify-content: center;
    padding: 0.2rem;
  }

  .cr_div1 img {
    width: 100%;
  }

  .cr_div2 {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 6;
    align-items: center;
    justify-content: center;

  }

  .cr_div2 input {
    width: 100%;
    height: 80%;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 0;
    margin-bottom: 0;
  }
</style>
