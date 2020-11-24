<template>
<div id =app>
    <div class="calculator">
        <div class="screen_container">
            <div class="display">{{expression}}</div>  
             <div class="answer">{{current}}</div>
        </div>
        <div class="grid-container">
              <button @click="percent"  class="grid-item " id=btn_operator>&#37;</button>
               <button @click="clear" class="grid-item" id="btn_operator">CE</button>
               <button @click="clear" class="grid-item" id="btn_operator">C</button>  
               <button @click="del" class="grid-item" id="btn_operator">del</button>
               <button @click="oneOver" class="grid-item" id="btn_operator">1/x</button>
               <button @click="square" class="grid-item" id="btn_operator">x<sup>2</sup></button>  
              <button @click="squareroot" class="grid-item" id="btn_operator">&#8730;<span style="text-decoration:overline;">&nbsp;X&nbsp;</span></button>
               <button @click="divide" class="grid-item" id="btn_operator">&#247;</button>
               <button @click="append('7')" class="grid-item" >7</button>
               <button @click="append('8')" class="grid-item">8</button>
               <button @click="append('9')" class="grid-item">9</button> 
               <button @click="times" class="grid-item" id="btn_operator">&#215;</button>
              <button @click="append('4')" class="grid-item">4</button>
               <button @click="append('5')" class="grid-item">5</button> 
               <button @click="append('6')" class="grid-item">6</button>
               <button @click="subtract" class="grid-item" id="btn_operator">&#8722;</button>
                <button @click="append('1')" class="grid-item">1</button>
               <button  @click="append('2')" class="grid-item">2</button>
               <button @click="append('3')" class="grid-item">3</button>  
               <button @click="add" class="grid-item" id="btn_operator">&#43;</button>
               <button @click="negate" class="grid-item">+/-</button>
               <button @click="append('0')" class="grid-item">0</button>
               <button @click="append('.')" class="grid-item">.</button>
               <button @click="equal" class="grid-item" id="btn_operator">&#61;</button>
             
              
        </div>
        
    </div>
 </div>
</template>


<script>
import axios from "axios";
export default{
  name:"home",
  data(){
    return{
      expression:'', 
     previous: '',
     current:'',
     link:'',
     operatorclicked: false,
     equaled:false
       

    };
  },
  methods:{
    append:function (number) {
      if(this.current=="can't divide by zero"||this.current=="NaN"){
        console.log("heloo");
        this.current=number;
        this.previous='';
         this.expression='', 
         this.operatorclicked= false,
          this.equaled=false
      }
      else{
          if(!(this.operatorclicked||this.equaled)){
         this.current += number;
         
      }
      else{
       if(this.equaled){
         this.expression='';
         this.previous='';
         this.equaled=false;
       }else{
         this.previous=this.current;
       }

        this.current=number;
        this.operatorclicked=false;
          console.log(this.previous);
      }
      }
         
        
    },

   equal:function(){
     if(!this.equaled){
     if(this.operatorclicked){
       this.previous=this.current;
       this.operatorclicked=false;
     }
     if(this.current==''){
      
       this.current=this.previous;
        
     }
       this.equaled=true;
        
        this.expression+=this.current+'=';
        this.getResult(this.previous,this.current,this.url);
       this.previous ="";
        console.log(this.previous);
     }
   
   },

    clear:function(){
         this.previous='';
         this.current='';
         this.expression='';
         this.operatorclicked= false;
    },
    del:function(){
      this.current=this.current.slice(0, -1);
    },

    operate:function(operator){
          if(this.equaled){
            this.expression=this.current+operator;
             this.previous='';
             this.equaled=false;
          }
          else{
             this.expression +=this.current+operator;
          }
          this.operatorclicked=true;
           if(!this.previous==''||this.previous=='0'){ 
              this.getResult(this.previous,this.current,this.url);
           }
           
    },

     times:function(){
         if(this.operatorclicked){
          if(this.expression.slice(-1)!="*"){
                 
                 this.url="http://localhost:8085/times";
                 this.expression=this.expression.slice(0, -1)+'*';
          }
        }
          else{
           if(!this.current==''||this.current=='0'){
            this.operate('x'); 
           this.url="http://localhost:8085/times";
         
           }
          }
           
    },
     divide:function(){
         if(this.operatorclicked){
          if(this.expression.slice(-1)!="/"){
                
                 this.url="http://localhost:8085/divide";
                 this.expression=this.expression.slice(0, -1)+'/';
          }
        }
          else{
         
           if(!this.current==''||this.current=='0'){
           this.operate('/'); 
           this.url="http://localhost:8085/divide";
          
           }
         }
    },
    subtract:function(){

        if(this.operatorclicked){
          if(this.expression.slice(-1)!="-"){
                 
                 this.url="http://localhost:8085/subtract";
                 this.expression=this.expression.slice(0, -1)+'-';
          }
        }
          else{
       
                  if(!this.current==''||this.current=='0'){
                      this.operate('-'); 
                      this.url="http://localhost:8085/subtract";
          }
        
          }
           
    },

     

    add:function(){
        if(this.operatorclicked){
          if(this.expression.slice(-1)!="+"){
               
                 this.url="http://localhost:8085/add";
                 this.expression=this.expression.slice(0, -1)+'+';
          }
        }
          else{
              
                if(!this.current==''||this.current=='0'){
                  
                this.operate('+'); 
                this.url="http://localhost:8085/add";
         }
         
          } 
 
    },

    square:function(){
      this.getResult2(this.current,"http://localhost:8085/square")

    },

    percent:function(){
      this.getResult2(this.current,"http://localhost:8085/percent")

    },
    oneOver:function(){
      this.getResult2(this.current,"http://localhost:8085/oneOver")

    },
    squareroot:function(){
      this.getResult2(this.current,"http://localhost:8085/squareroot")

    },
     negate:function(){
      this.getResult2(this.current,"http://localhost:8085/negate")

    },
  
    getResult(value1,value2,url){
        
          axios.get(url,{params:{
          firstNumber:value1,
          secondNumber:value2}
          })
          .then(function(response){
            this.current=response.data;
            
          }.bind(this))
         
 
  },

     getResult2(value1,url){
        
          axios.get(url,{params:{
          firstNumber:value1,
          }
          })
          .then(function(response){
            this.current=response.data;
            
          }.bind(this))
         
 
  }

  }
}




</script>


<style>

#app{
background-color:#ffe6ff;
height: 600px;
width:450px;

}

#btn_operator{
background-color:  #e600e6;
}
.screen_container{
 height: 140x;
padding: 10px;
}
.answer{
  height: 40px;
  border-bottom: 1px solid #800080;
}
.display{
height: 100px;
	
}


.grid-container {
  display: grid;
  grid-template-columns: auto auto auto auto;
  background-color: #ffe6ff;
 
  padding: 10px;
  
}
.grid-item {
  background-color:#ffb3ff;
  border: 1px solid white;
  padding: 18px;
  font-size: 30px;
  text-align: center;
  
}


</style>