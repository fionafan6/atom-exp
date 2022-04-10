<template>
  <div class="container" :style="`background-image: url('${this.imageList[currentIndex].url}')`">
  <!-- <div class="container" :style="`background-image: url('http://pv707742q.bkt.clouddn.com/BK_F1.png')`"> -->
    <!--<img class="background-image" :src="this.imageList[this.currentIndex]" />-->
    <div class="transparent-float">
      <div class="columns first">
        <div class="rows first" @click="rankImageLevel(1)"></div>
      </div>
      <!-- 中间部分为无响应区 fortest-->
      <div class="columns middle">
        <div class="rows"></div>
      </div>
      <div class="columns second">
        <div class="rows first" @click="rankImageLevel(2)"></div>
      </div>
    </div>
    <!--当前选择-->
    <div :class="['hint']">
      <div class="">
        {{this.imageList.length - this.currentIndex}}<br>
        {{this.scene}}<br>
        {{this.atom}}
      </div>
    </div>
    <div :class="['exphint1']">
      <div class="">
        {{this.exphint1}}
      </div>
    </div>
    <div :class="['exphint2']">
      <div class="">
        {{this.exphint2}}
      </div>
    </div>
    <!-- 黑屏 -->
    <div :class="['blackscreen',{'hidden': !this.isBlack}]">
    </div>
    <!--上一张-->
    <div class="lastImage" @click="lastImage">
      <!-- hidden: 为true的时候隐藏按钮 -->
      <a :class="['button',{'hidden': lasthideHint || this.isFirst || this.isBlack }]">上一张</a>
    </div>
    <!--下一张-->
    <div class="nextImage" @click="nextImage">
      <a :class="['button',{'hidden': nexthideHint || this.isLast || this.isBlack }]">下一张</a>
    </div>
    <!--提交-->
    <div class="nextImage" @click.stop="submitResult">
      <a :class="['button',{'hidden': !this.isLast}]">提交</a>
    </div>
    <!--结束框-->
    <div :class="['ending',{'hidden': this.hideEnd}]">
      {{this.finishhint}}
    </div>
    <b-loading :is-full-page="true" :active.sync="isLoading" :can-cancel="false"></b-loading>
  </div>
</template>
<script>
import { IMAGE_LIST } from '../data/imageList'
export default {
  name: 'HelloWorld',
  data () {
    return {
      lasthideHint: false,
      nexthideHint: true,
      userComment: '',
      List: IMAGE_LIST,
      currentIndex: 0,
      isFirst: true,
      isLast: false,
      hideEnd: true,
      userCommentList: [],
      imageList:IMAGE_LIST,
      randomList:[],
      response: '',
      isLoading: false,
      submitted: false,
      raceimage:[],
      isBlack: false,
      exphint1: '是',
      exphint2: '否',
      seq: [],
      atom: '',
      atomarr: [],
      imgnum: '',
      wordnum: '',
      imgname: '',
      expnum: '',
      scene: '',
      artimage: '',
      finishhint: '谢谢您的参与！即将开始下一组实验'
    }
  },
  mounted () {
      let userinfo = [{respones: 2, name: this.$route.query.name.concat('ready'), age: this.$route.query.num, pic: 'ready',url:'ready'}]
      console.log(userinfo)
      let promise0 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: userinfo
          });
      Promise.all([promise0]).then((res) => {
          console.log('success')
      }).catch((e) => {
          self.$toast.open({
              message: e || '服务器错误',
              type: 'is-danger'
          })
      })

    var _this = this;        
    document.onkeydown = function(e) {            
      let key = window.event.keyCode;            
      if (key== 37) {
            window.event.preventDefault()
            _this.rankImageLevel(1);
      }
      if (key== 39) {
      window.event.preventDefault()
      _this.rankImageLevel(2);
      }
    };
    this.expnum = this.$route.query.num;
    console.log(this.expnum)
    switch(this.expnum){
      case 1:
        this.atom = '发生错误，请联系助教'
        break;
      case 2:
        this.atom = '是否喜欢该图像' 
        this.finishhint = '感谢您的参与'
        break;
    }
  
    switch(this.expnum){
      case '1':
        this.atom = '发生错误，请联系助教'
        break;
      case '2':
        this.atom = '是否喜欢该图像'
        this.finishhint = '感谢您的参与'
        break;
    }
    
    console.log(this.expnum)
    // 初始化数据
    this.currentIndex = 0
    this.isFirst = true
    this.isLast = false
    // this.hideEnd = true
    // 随机打乱数组
    this.seq = [];
    for(let i = 0; i < 19; i++) this.seq[i] = i;
    this.seq = this.seq.sort((a, b) => {
      return Math.random() > (0.5) ? -1 : 1
      })
    //40*49张图
    for (let race = 0; race < 19; race++){
      this.raceimage = this.List.slice(16 * this.seq[race], 16 * this.seq[race] + 16)
      this.randomList = this.randomList.concat(this.raceimage.sort((a, b) => {
      return Math.random() > (0.5) ? -1 : 1
      }))
    }
    this.imageList = this.randomList
    console.log(this.imageList)
    this.imgname = this.imageList[this.currentIndex].pic.split('_')
    this.imgnum = this.imgname[0]
    this.wordnum = this.imgname[1]  // word。
    // console.log(this.imageList)

    switch(this.imgnum){
      case '1':
        this.scene = '安中大楼'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
      case '2':
        this.scene = '二月兰'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '3':
        this.scene = '拱桥'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '4':
        this.scene = '树林'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：生机'
              break;
          }
        }
        break;
             case '5':
        this.scene = '图书馆'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：现代'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
             case '6':
        this.scene = '郁金香'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：活泼'
              break;
            case '2':
              this.atom = '氛围感：热烈'
              break;
            case '3':
              this.atom = '氛围感：鲜艳'
              break;
          }
        }
        break;
    }

    

  },
  methods: {
    rankImageLevel (number) {
      let userComment = number;
      // switch (number) {
      //   case 1:
      //     userComment = '非常喜欢'
      //     break
      //   case 2:
      //     userComment = '喜欢'
      //     break
      //   case 3:
      //     userComment = '有点喜欢'
      //     break
      //   case 4:
      //     userComment = '有点不喜欢'
      //     break
      //   case 5:
      //     userComment = '不喜欢'
      //     break
      //   case 6:
      //     userComment = '非常不喜欢'
      //     break
      // }
      //this.hideHint = false;
      this.userComment = userComment;
      this.response = number
      this.nextImage()
    },
    nextImage () {
      let userComments = this.userCommentList
      if (userComments.length < this.imageList.length){
        userComments.push(this.response)
      }
      
      this.userCommentList = userComments
      let length = this.imageList.length
      console.log(length)
      if (this.currentIndex < length) {
        this.currentIndex++

      }
      this.isLast = this.currentIndex === length
      this.isFirst = this.currentIndex === 0
      if (this.isLast){this.currentIndex--}
      this.imgname = this.imageList[this.currentIndex].pic.split('_')
      this.imgnum = this.imgname[0]
      this.wordnum = this.imgname[1]  // word
         switch(this.imgnum){
      case '1':
        this.scene = '安中大楼'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
      case '2':
        this.scene = '二月兰'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '3':
        this.scene = '拱桥'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '4':
        this.scene = '树林'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：生机'
              break;
          }
        }
        break;
             case '5':
        this.scene = '图书馆'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：现代'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
             case '6':
        this.scene = '郁金香'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：活泼'
              break;
            case '2':
              this.atom = '氛围感：热烈'
              break;
            case '3':
              this.atom = '氛围感：鲜艳'
              break;
          }
        }
        break;
    }
        if (this.currentIndex%16==0 && this.expnum==1){
          alert('下面进入下一词语（请参考左上方指示的词语）')
        }
      console.log(this.userCommentList)
      //this.hideHint = true
    },
    lastImage () {
      let userComments = this.userCommentList
      userComments.splice(-1,1)
      this.userCommentList = userComments
      if (this.currentIndex > 0) {
        this.currentIndex--
      }
      this.isFirst = this.currentIndex === 0
      this.isLast = this.currentIndex === length
      console.log(this.userCommentList)
      this.imgname = this.imageList[this.currentIndex].pic.split('_')
      this.imgnum = this.imgname[0]
      this.wordnum = this.imgname[1]  // word
        switch(this.imgnum){
      case '1':
        this.scene = '安中大楼'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
      case '2':
        this.scene = '二月兰'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '3':
        this.scene = '拱桥'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：古典'
              break;
            case '3':
              this.atom = '氛围感：雅致'
              break;
          }
        }
        break;
      case '4':
        this.scene = '树林'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：神秘'
              break;
            case '3':
              this.atom = '氛围感：生机'
              break;
          }
        }
        break;
             case '5':
        this.scene = '图书馆'
        if (this.expnum==1) {
            switch(this.wordnum){
            case '1':
              this.atom = '氛围感：安静'
              break;
            case '2':
              this.atom = '氛围感：现代'
              break;
            case '3':
              this.atom = '氛围感：庄严'
              break;
          }
        }
        break;
             case '6':
        this.scene = '郁金香'
        if (this.expnum==1) {
                    switch(this.wordnum){
            case '1':
              this.atom = '氛围感：活泼'
              break;
            case '2':
              this.atom = '氛围感：热烈'
              break;
            case '3':
              this.atom = '氛围感：鲜艳'
              break;
          }
        }
        break;
    }
      //this.hideHint = true
    },
    submitResult () {
      if(this.isLoading){
        return;
      }
      if(this.submitted){
        this.$toast.open({
          message: '数据已经提交成功，请勿重复提交',
          type: 'is-danger'
        })
        return ;
      }
      //上传实验数据
      this.isLoading = true;
      let userComments = this.userCommentList
      let self = this;
      if (userComments.length < this.imageList.length){
        userComments.push(this.response)
      }
      this.userCommentList = userComments
      // this.hideEnd = false

      let picList = this.imageList
      for (let i = 0; i < picList.length; i++) {
        picList[i].response = userComments[i]
        picList[i].name = this.$route.query.name.concat(this.$route.query.num)
        picList[i].age = this.$route.query.age
      }
      console.log(picList)


      //分割实验数据
      let length = picList.length;
      let perListLength = length / 5;
      let picList1 = picList.slice(0,perListLength);
      let picList2 = picList.slice(perListLength,perListLength*2);
      let picList3 = picList.slice(perListLength*2,perListLength*3);
      let picList4 = picList.slice(perListLength*3,perListLength*4);
      let picList5 = picList.slice(perListLength*4,perListLength*5);

      let promise1 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: picList1
          });
      let promise2 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: picList2
          });
      let promise3 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: picList3
          });
      let promise4 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: picList4
          });
      let promise5 = axios.post('http://120.79.147.66:8081/survey/submit',{
              data: picList5
          });
      Promise.all([promise1,promise2,promise3,promise4,promise5]).then((res) => {
          self.isLoading = false;
          self.hideEnd = false;
          self.submitted = true;
          // this.$toast.open({
          //     message: '提交成功'
          // })
      }).catch((e) => {
          self.isLoading =false;
          self.$toast.open({
              message: e || '服务器错误',
              type: 'is-danger'
          })
      })
      if (this.$route.query.num != 2){
        setTimeout(() => {
            this.$router.push({path: '/userinfo', query: {name: this.$route.query.name, age: this.$route.query.age, num: parseInt(this.$route.query.num)+1}})
          }, 3000)
      }


      // axios.post('http://120.79.147.66:8081/survey/submit', {
      //   data: picList
      // }).then(function (response) {
      //   console.log(response)
      //   self.isLoading = false;
      //   self.hideEnd = false;
      // }).catch(function (error) {
      //   self.isLoading = false;
      //   self.$toast.open({
      //     messgae: error.messgae || '服务器未知错误,请稍后重新提交',
      //     type: 'is-danger',
      //   })
      // })
    }
  }
}
</script>
<style lang="less" scoped>
.container {
  width: 100vw;
  height: 100vh;
  max-width: 100vw;
  max-height: 100vh;
  background-color: black;
  background-image: url("https://wx4.sinaimg.cn/mw690/007xZLPUly1fyzc5f36mbj30k00dcgmy.jpg");
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  /*background-size: 100% 100%;*/
  .background-image {
    height: 100vh;
    width: 100vw;
  }
  .transparent-float {
    position: absolute;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    max-width: 100vw;
    max-height: 100vh;
    background-color: transparent;
    display: flex;
    .columns {
      flex: 1;
      background-color:transparent;
      display: flex;
      flex-direction: column;
      .rows {
        flex: 1;
        background-color: transparent;
      }
    }
    .middle {
      width: 25vw
    }
  }
  .blackscreen {
  position: absolute;
  height: 100vh;
  width: 100vw;
  background-color: black;
    &.hidden {
      display: none;
    }
  }
  .hint {
    position: absolute;
    top: 20px;
    left: 20px;
    border-radius: 4px;
    padding: 10px;
    background: rgb(128,128,128);
    /*transition-property: all;*/
    /*transition-duration: 1s;*/
    opacity: 1;
    visibility: visible;
    /*transition: all 0.8s linear;*/
    font-weight: bold;
    color: black;

    &.hidden {
      opacity: 0;
      visibility: hidden;
    }
    span {
      font-weight: bold;
      color: brown;
    }
  }
  .exphint1 {
    position: absolute;
    bottom: 50%;
    left: 20px;
    border-radius: 4px;
    padding: 10px;
    background: rgb(128,128,128);
    /*transition-property: all;*/
    /*transition-duration: 1s;*/
    opacity: 1;
    visibility: visible;
    /*transition: all 0.8s linear;*/
    font-weight: bold;
    color: rgb(0, 0, 0);
  }
  .exphint2 {
    position: absolute;
    bottom: 50%;
    right: 20px;
    border-radius: 4px;
    padding: 10px;
    background: rgb(128,128,128);
    /*transition-property: all;*/
    /*transition-duration: 1s;*/
    opacity: 1;
    visibility: visible;
    /*transition: all 0.8s linear;*/
    font-weight: bold;
    color: rgb(0, 0, 0);
  }
  .lastImage {
    position: absolute;
    left: 10px;
    bottom: 20px;
    background: rgb(128,128,128);
  }
  .nextImage {
    position: absolute;
    right: 10px;
    bottom: 20px;
    background: rgb(128,128,128);
  }
}
.button {
  background-color: rgba(255,255,255,0);
  padding: 10px;
  opacity: 1;
  visibility: visible;
  border-color: black;
  font-weight: bold;
  color: black;
  /*transition: all 0.8s linear;*/
  &.hidden {
    display: none;
  }
}
.ending {
  position: absolute;
  height: 100vh;
  width: 100vw;
  background-color: rgba(255,255,255,0.8);
  z-index: 99;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5rem;
  font-weight: bold;
  color: brown;
  &.hidden {
    display: none;
  }
}
</style>
