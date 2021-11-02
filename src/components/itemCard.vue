<template>
  <div class="CardFlexContainer" >
           <div  class="CardFlexContainer-Card Card"  v-for="item in ProductObject" v-bind:key="item.id">
            <img
              :src="item.image.file.url"
              class="Card-Img"
              alt="Название"
            />
               <h5 class="Card-Title" >{{item.title}}</h5>
              <h6 class="Card-Subtitle">{{item.price}}</h6>
              <a href="#" class="Card-Link" v-for="color in item.colors" v-bind:key="color.id" :style="{color:color.code}">{{color.title}}</a>
          </div>
         
      </div>  
</template>
<script>
import axios from "axios";



export default {

    name:'itemCard',
    data(){
return{
    ProductObject:null,
}
    },

       

mounted(){
axios
  .get("https://vue-study.skillbox.cc/api/products", {
    params: {
      categoryId: 3,
    },
  })
  .then((response) => {
    this.ProductObject=response.data.items;
   }).catch(err=>alert(err+' Оплатите инернет111!'));
},

       
 }
</script>

<style lang="css" scoped>
.CardFlexContainer{

    display: flex;
    flex-wrap: wrap;
    width:100%;
    justify-content: space-between;

}
.Card{
    width:49%;
    height: 400px;
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    margin-top: 5vh;
    justify-content: space-evenly;
    align-items: center;
    box-shadow:6px 6px 17px 0px;
 transition: all 0.5s;
 }
.Card:hover{
     box-shadow:6px 6px 27px 0px;    
   
}
.Card-Img{
 width:200px;
 height: 200px;
 object-fit: contain;
    }
    .Card-Subtitle{color:rgb(245, 183, 51)}
 .Card-Title,.Card-Subtitle,.Card-Price{
      text-align: center;
     
     }
     
    @media (min-width:320px) and (max-width:500px){
       
       .CardFlexContainer{
           align-items: center;
           flex-direction: column;
       }

        .Card{
            width:80%;
        }
       
     .Card-Link{
         margin-left: 10px;
     }
    }

</style>
