
<template>
  <div class="container">
    <div :class="['info',{'hidden':blackScreen}]">
      <div :class="['info-input',{'hidden':this.showHint}]">
        <div class="name">
          <span>姓名:</span>
          <input type="text" v-model="name" />
        </div>
        <div class="age">
          <span>年龄:</span>
          <input type="text" v-model="age"/>
        </div>
        <div class="num">
          <span>序号:</span>
          <input type="text" v-model="num"/>
        </div>
        <div class="begin">
          <div class="button" @click="beginTest">Start</div>
        </div>
      </div>
      <div :class="['hint',{'hidden': !showHint}]">
        即将进入视觉适应阶段，此阶段为30s黑屏<br>
        The black screen will last 30s for visual adaptation. Please wait.
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      name: '',
      age: '',
      num: '',
      showHint: false,
      blackScreen: false,
    }
  },
  mounted () {
    this.showHint = false
    this.blackScreen = false

    if (this.$route.query.num){
      this.num = this.$route.query.num;
      this.name = this.$route.query.name;
      this.age = this.$route.query.age;
    }
      
  },
  methods: {
    beginTest () {
      if (this.name && this.age && (this.num == 1||this.num == 2) && (this.age.length == 2 || this.age.length == 1)) {
        this.showHint = true
        console.log(this.num)
      /*let userinfo = [{respones: 2, name: this.name.concat(this.num.concat('ready')), age: this.age, pic: 'ready',url:'ready'}]
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
      })*/

        setTimeout(() => {
          this.blackScreen = true
        }, 3000)
        setTimeout(() => {
          this.$router.push({path: '/image', query: {name: this.name, age: this.age, num: this.num}})
        }, 3000)
      } else {
        alert('姓名、年龄或序号有误！')
      }
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
  display: flex;
  justify-content: center;
  align-items: center;
  .info {
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
    background-color: rgba(244,239,225);
    color:#df6426;
    &.hidden {
      display: none;
    }
    .info-input {
      &.hidden {
        display: none;
      }
      .name {
        margin-bottom: 10px;
        font-weight: bold;
      }
      .age {
        font-weight: bold;
        margin-bottom: 10px;
      }
      .num {
        font-weight: bold;
        margin-bottom: 10px;
      }
      .begin {
        display: flex;
        justify-content: center;
        .button {
          padding: 5px;
          text-align: center;
          width: 80px;
          color: #ffffff;
          background-color: #df6426;
          border-radius: 4px;
        }
      }
    }
  }
  .hint {
    color: #df6426;
    &.hidden {
      display: none;
    }
  }
}
input {
  background-color: rgba(245,230,218);
  border: 1px solid rgba(232,200,178);
  padding: 5px;
  border-radius: 2px;
  margin-left: 10px;
  color: #a0806b;
}
</style>

