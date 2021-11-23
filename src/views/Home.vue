<template>
  <ion-page class="outer" >
    <div class="inner">
      <div class="output outer">
        <div class="inner number">
          <h1>{{input}}</h1>
        </div>
      </div>
      <div class="outer">
        <div class="line inner"></div>
      </div>
      <div class="bottom">
        <div class="container ">
          <ion-button class="dobbleButton" @click="clear()">AC</ion-button>
          <ion-button @click="deleteChar()">DEL</ion-button>
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
          <ion-button class="dobbleButton" @click="calculate()">=</ion-button>
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
      afterCalculate:false,
      error:false,
      numKey:[{code:'Numpad0',key:'0'},{code:'Numpad1',key:'1'},{code:'Numpad2',key:'2'},{code:'Numpad3',key:'3'},{code:'Numpad4',key:'4'},{code:'Numpad5',key:'5'},{code:'Numpad6',key:'6'},{code:'Numpad7',key:'7'},{code:'Numpad8',key:'8'},{code:'Numpad9',key:'9'},{code:'NumpadDecimal',key:'.'},],
      operationKey:[{code:'NumpadAdd',key:'+'},{code:'NumpadSubtract',key:'-'},{code:'NumpadMultiply',key:'*'},{code:'NumpadDivide',key:'/'},]
    }
  },
  mounted() {
    window.addEventListener('keydown', this.keyPress);
  },
  unmounted() {
    window.removeEventListener('keydown', this.keyPress);
  },
  methods:{
    keyPress(e) {
      const cmd = e.code;
      console.log(cmd)
      this.numKey.forEach((value, index) => {
        if( cmd == value.code )
        {
          this.addNumber(value.key)
        }
      });
      this.operationKey.forEach((value, index) => {
        if( cmd == value.code )
        {
          this.addOperation(value.key)
        }
      });
      if( cmd == 'Enter')
      {
        this.calculate()
      }
      if( cmd == 'Backspace')
      {
        this.deleteChar()
      }
      if( cmd == 'Delete')
      {
        this.clear()
      }
    },
    addNumber(char){
      if( this.input == '0' )
      {
        this.input = ''
      }else if( this.afterCalculate )
      {
         this.input = ''
         this.afterCalculate = false
      }

      this.error = false
      this.input += char
      this.chcekLength()
    },
    addOperation(char){
      if( !this.error )
      {
        if( this.afterCalculate )
        {
          this.afterCalculate = false
        }
        this.input += char
        this.chcekLength()
      }
    },
    clear(){
      this.input = '0'
      this.error = false
    },
    calculate(){
      let fine = true
      try {
        eval(this.input); 
      } catch (error) {
        if (error instanceof SyntaxError)  
        {
          fine = false
          this.input="syntax error"
          this.error = true
        }
      }finally{
        if(fine)
        {
          this.input = eval(this.input)
          this.chcekLength()
        }
      }
      this.afterCalculate = true
    },
    deleteChar(){
      if( this.error )
      {
        this.input = '0'
        this.afterCalculate = false
        this.error = false
      }

      if( this.afterCalculate == true )
      {
        this.afterCalculate = false
      }
      else
      {
        if( this.input != '0' )
        {
          this.input = this.input.slice(0, -1)
          if( this.input == '' )
          {
            this.input = '0'
          }
        }
      }
      this.chcekLength()
    },
    chcekLength()
    {
      if( this.input.length > 47 )
      {
        this.input = "To long"
        this.afterCalculate = true
        this.error = true
      }
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
  height: 1px;
  width: 98%;
  background-color: var(--ion-color-medium	);
}

.output{
  height: 40vw;
}

.number{
  width: 90%;
  word-break: break-all;
}
.number h1{
  font-size: 9.25vw !important;
}
.dobbleButton{
  width: 44.5vw !important;
}
.bottom{

  width: 100%;
}
</style>