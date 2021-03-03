<template>
  <div id="display">
      <div id="displayText">
        <div id = "leftOf">{{displayValue}}</div>
      </div>
      <div id="subText"><p>{{subDisplayValue}}</p></div>
  </div>
</template>

<script>

export default {
  name: 'display',
  props: {
      displayValue: String,
      subDisplayValue: String
  },
  components:{
  },
  mounted:function(){
    this.onLoady()
  },
  methods: {
    onLoady: function() {
    }
  }
  
}
</script>

<style >
@import url('https://fonts.googleapis.com/css?family=Lato&display=swap');

#display {
  width: 100%;
  border-radius: 5px;
  height: 200px;
  float: right;
}

#leftOf{
  width: 99.75%;
}

#displayText,  #subText{
    font-family: 'Lato', sans-serif;
    text-align: right;
    margin-bottom: 0px;
    overflow:hidden;
    text-overflow: ellipsis;
}
#displayText{
    display: flex;
    flex-direction: row;
    height: 100px;
    font-size: 3em;
}

#subText{
    height: 100px;
    font-size: 1.5em;
    color: #38908f;
    border: none;
    background-color: #f9f9f9;
}
</style>
