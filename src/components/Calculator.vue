<template>
  <div id="calcWrapper">
      <Display v-bind:displayValue=dispValue v-bind:subDisplayValue=subDispValue></Display>
      <Button   v-for="b in buttons" 
                v-bind:key="b.id" 
                v-bind:bValues="b"
                v-on:handle-press="handlePress" 
                v-bind:style="b.id === 17 ? bStyleObjectLarge:  bStyleObjectRegular">
      </Button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Display from './Display.vue'
import Button from './Button.vue'

export default Vue.extend({
  data: function ()  {
      return {
          dispValue: "0",
          subDispValue: "",
          prevValue: "0",
          prevOp: "=",
          opInEffect: false,
          bStyleObjectRegular:{
              width: '25%'
          },
          bStyleObjectLarge:{
              width: '49.9%',
          },
          buttons:[
              {
                  id: 1,
                  name: "AC",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 2,
                  name: "\u00b1",
                  isReg: true,
                  isOp: true
              },
              {
                  id: 3,
                  name: "%",
                  isReg: true,
                  isOp: true
              },
              {
                  id: 4,
                  name: "/",
                  isReg: false,
                  isOp: true
              },
              {
                  id: 5,
                  name: "7",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 6,
                  name: "8",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 7,
                  name: "9",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 8,
                  name: "x",
                  isReg: false,
                  isOp: true
              },
              {
                  id: 9,
                  name: "4",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 10,
                  name: "5",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 11,
                  name: "6",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 12,
                  name: "-",
                  isReg: false,
                  isOp: true
              },
              {
                  id: 13,
                  name: "1",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 14,
                  name: "2",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 15,
                  name: "3",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 16,
                  name: "+",
                  isReg: false,
                  isOp: true
              },
              {
                  id: 17,
                  name: "0",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 18,
                  name: ".",
                  isReg: true,
                  isOp: false
              },
              {
                  id: 19,
                  name: "=",
                  isReg: false,
                  isOp: true
              }
          ]
      }
  },
  components:{
      Display,
      Button
  },
  methods: {
      handlePress: function (event: { textContent: string }){
          var number = event.textContent.trim();
          switch (number){
              case "AC": this.clearAll();
                break;
              case "0":
              case "1":
              case "2":
              case "3":
              case "4":
              case "5":
              case "6":
              case "7":
              case "8":
              case "9": this.numberPressed(number);
              break; 
              case "+": 
              case "-": 
              case "/": 
              case "x": this.computeOp(number);
                break;
              case "=": this.equalPressed();
                break;
              case ".": this.addPoint();
                  break;
              case "\u00b1": this.negateValue();
                  break;
              default:
                  alert("KEY ERROR: in default");
          }
      },
      negateValue: function(): void {
          if (this.dispValue != "0"){
              if (this.dispValue.indexOf("-") < 0){
              this.dispValue = "-" + this.dispValue;
            }
            else{
                this.dispValue = this.dispValue.substring(1);
            }
          }
          
      },
      addPoint: function() {
          if (this.dispValue.indexOf(".") < 0){
              this.dispValue += "."
          }
      },
      equalPressed: function(){
          try{
              this.computeEqual(this.prevValue, this.dispValue, this.prevOp);
              this.subDispValue = "";
          }
          catch (e){
              alert(e);
          }
      },
      numberPressed: function(number: string){
          this.opInEffect = false;
          if (this.dispValue === "0"){
              this.dispValue = number;
          }
          else {
              if (this.dispValue.length >= 15){
                  alert("KEY ERROR: Display limit reached");
              }
              else{
                  this.dispValue += number;
              }
              
          }
          
      },
      computeOp: function (op: string){
          if (!this.opInEffect){
            try{
                this.computeEqual(this.prevValue, this.dispValue, this.prevOp);
                this.prevValue = this.dispValue;
                this.dispValue = "0";
                this.prevOp = op;
                this.subDispValue = this.prevValue.toString() + " " + this.prevOp + "    ";
                this.opInEffect = true;
            }
            catch (e){
                alert(e);
            }
            
          }
          
      },
      computeEqual: function(op1: string, op2: string, op: string){
          let op1_num = parseFloat(op1);
          let op2_num = parseFloat(op2);
          let result = 0;
          if (op === "+"){
              result = op1_num + op2_num;
          }
          else if (op === "-"){
              result = op1_num - op2_num;
          }
          else if (op === "x"){
              result = op1_num * op2_num;
          }
          else if (op === "/"){
              if (op2_num == 0){
                  throw "MATH ERROR: Cannot divide by 0";
              }
              else{
                  result = op1_num / op2_num;
              }
              
          }
          else{
              result = op2_num;
          }
          let temp = result.toString();
          if (temp.length >= 15){
              throw "DISPLAY ERROR: Computation result will not fit on display. Use C or AC to perform a simpler computation.";
          }
          this.dispValue = temp;
          this.prevValue = "0";
          this.prevOp = "=";

      },
      clearDisplay: function (){
          this.dispValue = "0";
      },
      clearAll: function (){
          this.clearDisplay();
          this.prevValue = "0";
          this.prevOp = "=";
          this.subDispValue ="";
          this.opInEffect = false;
      }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#calcWrapper {
  width: 100%;
  margin: auto;
  box-sizing: border-box;
  border: 2px solid black;
  border-radius: 5px;
  height: 70%;
  overflow: hidden;
}

@media screen and (min-width: 768px){
    #calcWrapper {
        width: 100%;
        margin: auto;
    }
}
</style>
