<template>
  <ion-page class="outer">
    <div class="inner">
      <div class="output outer">
        <div class="inner number">
          <h1 v-bind:style="{fontSize: fontSize + 'vw' }">{{input}}</h1>
        </div>
      </div>
      <div class="outer">
        <div class="line inner"></div>
      </div>
      <div>
         <div class="container ">
          <ion-button class="dobbleButton" @click="clean()">AC</ion-button>
          <ion-button @click="deleteFunction()">DEL</ion-button>
          <ion-button @click="addOperation('+')">+</ion-button>
        </div>
        <div class="container">
          <ion-button @click="addNumber('7')">7</ion-button>
          <ion-button @click="addNumber('8')">8</ion-button>
          <ion-button @click="addNumber('9')">9</ion-button>
          <ion-button @click="addOperation('-')">-</ion-button>
        </div>
        <div class="container ">
          <ion-button @click="addNumber('4')">4</ion-button>
          <ion-button @click="addNumber('5')">5</ion-button>
          <ion-button @click="addNumber('6')">6</ion-button>
          <ion-button @click="addOperation('*')">*</ion-button>
        </div>
        <div class="container ">
          <ion-button @click="addNumber('1')">1</ion-button>
          <ion-button @click="addNumber('2')">2</ion-button>
          <ion-button @click="addNumber('3')">3</ion-button>
          <ion-button @click="addOperation('/')">/</ion-button>
        </div>
        <div class="container ">
          <ion-button @click="addNumber('0')">0</ion-button>
          <ion-button @click="addNumber('.')">.</ion-button>
          <ion-button class="dobbleButton" @click="eaquals()">=</ion-button>
        </div>
      </div>
    </div>
  </ion-page>
</template>

<script >
import { IonPage, IonButton } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: {
    IonPage,
    IonButton,
  },
  data(){
    return{
      input:'0',
      afterEaquals:false,
      fontSize: 20,
      minFontSize: 20,
      maxFontSize: 20
    }
  },
  methods:{
    addNumber(char){
      if(this.input=='0'){
        this.input=''
      }else if(this.afterEaquals){
         this.input=''
         this.afterEaquals=false
      }
      this.input+=char
      this.setFontSize()
    },
    addOperation(char){
      if(this.afterEaquals){
         this.afterEaquals=false
      }
      this.input+=char
      this.setFontSize()
    },
    clean(){
      this.input='0'
      this.setFontSize()
    },
    eaquals(){
      this.input=eval(this.input)
      this.afterEaquals = true
      this.setFontSize()
    },
    deleteFunction(){
     
      if(this.afterEaquals == true){
        this.afterEaquals = false
      }else{
        if(this.input!='0'){
          this.input=this.input.slice(0, -1)
          if(this.input==''){
            this.input='0'
          }
        }
      }
      this.setFontSize()
    },
    setFontSize(){
      this.fontSize = this.maxFontSize - this.input.length/4 ;
      console.log(this.maxFontSize - this.input.length/4);
    }
  }
});
</script>

<style scoped>

.inner {
  display: table;
  margin: 0 auto;
  width: 98%;
  height: fit-content;
  max-height: 600px;
  text-align: right;
}

.outer {
  width:100%
}

ion-button{
  width: 22vw;
  height: 22vw;
  font-size: 5vW;
  font-weight: 600;
}

.container {
  display: flex;
  justify-content: center;
  margin: 0px;
}

.line{
  height: 2px;
  width: 98%;
  background-color: lightgray;
}

.output{
  height: 30vw;
}

.number{
  width: 90%;
}
.dobbleButton{
  width: 44.5vw !important;
}
</style>