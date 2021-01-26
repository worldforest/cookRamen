<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <input type="checkbox" id="파" value="파" v-model="checkIngredient">
    <label for="파">파</label>
    <input type="checkbox" id="계란" value="계란" v-model="checkIngredient">
    <label for="계란">계란</label>
    <input type="checkbox" id="김치" value="김치" v-model="checkIngredient">
    <label for="김치">김치</label>
    
    <div v-if="checkIngredient.length > 0" style="display: flex; align-items: center; justify-content: center;">
      <button @click="order()"><h4 v-if="!isOrdered">주문하기</h4><h4 v-else-if="isOrdered">취소하기</h4></button>
      <button v-if="isOrdered&&!isAsked" @click="ask()"><h4>문의하기</h4></button>
    </div>

    <div id="current"> </div>
    <div id="finish" v-if="this.isAsked"> </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      isOrdered: false,
      isAsked: false,
      startTime: null,
      askMin: null,
      askSec: null,
      finishSec: null,
      finishMin: null,
      timerId: null,
      finishTimer: null,
      checkIngredient: []
    };
  },
  props: {
    msg: String
  },
  methods: {
    check() {
      if(this.checkIngredient){
        this.isOrdered = !this.isOrdered;
      }
    },
    order() {
      this.isOrdered = !this.isOrdered;
      var now = new Date();
      this.startTime = now.getMinutes();
      this.askSec = now.getSeconds();

      if(this.isOrdered==true){
        this.timerId = setInterval(this.printLeftAskTime,1000);
        this.finishTimer = setInterval(this.printLeftFinishTime,1000);
        this.$alert("주문 시각은 "+now.getHours()+"시 "+this.startTime+"분 "+this.askSec+"초 입니다 :)");
        this.askMin=this.startTime+1;
        this.finishMin=this.startTime+3;
        this.finishSec=this.askSec;
      }
      else{
        clearInterval(this.timerId);
        this.$alert("주문이 취소되었습니다.");
        document.getElementById("current").innerHTML="";
        this.isAsked=false;
      }
    },
    ask() {
      var currtime = new Date();
      var min = this.askMin - currtime.getMinutes();
      var sec = (currtime.getSeconds() + this.askSec)%60;

      this.isAsked=true;
      var total = 120-(min*60+sec);
      var msg="";
      if(total <= 10){
        this.$alert("물을 받는 중입니다.");
      }else if(total <= 20){
        this.$alert("분말을 넣는 중입니다.");
      }else if(total <= 90){
        this.$alert("물을 끓이는 중입니다.");
      }else if(total > 90){
        this.$alert("면을 넣는 중입니다.");
      }
      document.getElementById("current").innerHTML="요리를 기다려주세요!";
      clearInterval(this.timerId);
    },
    finishAsk() {
      this.isAsked=true;
      clearInterval(this.timerId);
      this.$alert("문의 시간이 끝났습니다.");
      document.getElementById("current").innerHTML="요리를 기다려주세요!";
    },
    printLeftAskTime() {
      var currtime = new Date();
      var min = this.askMin - currtime.getMinutes();
      var sec = (60 - currtime.getSeconds() + this.askSec)%60;

      if(min==0 && sec==0){
        this.finishAsk();
      }else{
        document.getElementById("current").innerHTML="문의 가능 남은 시간 | " + this.addzero(min)+":"+this.addzero(sec);
      }
    },
    printLeftFinishTime() {
      var currtime = new Date();
      var min = this.finishMin - currtime.getMinutes();
      var sec = (60 - currtime.getSeconds() + this.finishSec)%60;
      document.getElementById("finish").innerHTML="요리 완성까지 남은 시간 | " + this.addzero(min)+":"+this.addzero(sec);
      if(min==0 && sec==0){
        this.isOrdered=false;
        clearInterval(this.finishTimer);
        this.$alert("라면이 완성되었습니다!");
      }
    },
    addzero(num) {
      return (num < 10 ? '0'+num : ''+num);
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h4 {
  font-size: 2.2vw;
}
button {
  display: flex; align-items: center; justify-content: center;
  width: 20vw;
  height: 8vw;
  margin: 2vw;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
label {
  font-size: 20px;
  padding: 20px;
}
</style>
