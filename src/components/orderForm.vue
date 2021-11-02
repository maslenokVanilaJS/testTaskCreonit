<template>
   <form @submit.prevent="postFormFields">
        <div class="form-group">
          <label class="d-block">
            <div class="mb-2">Представьтесь</div>
            <input type="text"   v-model.trim="order.name" class="form-control " :class="{'is-invalid':validate.invalidName||validate.notFoundUser}" />
                        <div class="invalid-feedback">{{this.Errors.name||this.Errors.query}}</div>

          </label>
        </div>
        <div class="form-group">
          <label class="d-block">
            <div class="mb-2">Адрес доставки</div>
            <input type="text" v-model.trim="order.address" class="form-control" :class="{'is-invalid':validate.invalidAddress}" />
                                    <div class="invalid-feedback">{{this.Errors.address}}</div>

          </label>
        </div>
        <div class="form-group">
          <label class="d-block">
            <div class="mb-2">Телефон</div>
            <input   maxlength="16"  v-model.trim="order.phone" :class="{'is-invalid':validate.invalidPhone}" class="form-control" />
                                             <div class="invalid-feedback">{{this.Errors.phone}}</div>

          </label>
        </div>
        <div class="form-group">
          <label class="d-block">
            <div class="mb-2">Эл. почта</div>
            <input   v-model="order.email" class="form-control" :class="{'is-invalid':validate.invalidEmail}" />
            <div class="invalid-feedback">{{this.Errors.email}}</div>

          </label>
        </div>
        <div class="form-group">
          <label class="d-block">
            <div class="mb-2">Комментарий</div>
            <textarea class="form-control" v-model.trim="order.comment"></textarea>
          </label>
        </div>
        <button type="submit"  class="btn btn-primary">Отправить</button>
      </form> 
      
</template>
<script>
import axios from "axios";

export default {
    name:'orderForm',
     
    data(){
      return{
order:{
  name:null,
  address:null,
  phone:null,
  email:null,
  comment:null,
  accessKey:null,
},
validate:{
  invalidName:false,
  invalidAddress:false,
  invalidPhone:false,
  invalidEmail:false,
  invalidComment:false,
  notFoundUser:false,
},
Errors:{
  name:null,
  phone:null,
  address:null,
  email:null,
  query:null,
},
      }
    },
    methods:{
      postFormFields(){

  axios({
   method:'post',
   url:'https://vue-study.skillbox.cc/api/orders',
   params:{accessKey:this.order.accessKey},
   data:{name:this.order.name,
  address:this.order.address,
  phone:this.order.phone,
  email:this.order.email,
  comment:this.order.comment,} 
  })
  .then(response=>console.log(response.error,'then'))
  .catch((error)=>{

    if (error.response.data.error.request!=null) {
      this.Errors.email=error.response.data.error.request.email;
  this.Errors.phone=error.response.data.error.request.phone;
  this.Errors.address=error.response.data.error.request.address;
  this.Errors.name=error.response.data.error.request.name;
    }else{
        this.Errors.email=null;
  this.Errors.phone=null;
  this.Errors.address=null;
  this.Errors.name=null;
  this.Errors.query=error.response.data.error.query.userAccessKey;

     }
      console.log(this.order.accessKey);

  }
  );


   
  

 },
 
    },
    updated(){
    (this.Errors.name!=null||''||undefined)?this.validate.invalidName=true:this.validate.invalidName=false;
    (this.Errors.address!=null||''||undefined)?this.validate.invalidAddress=true:this.validate.invalidAddress=false;
    (this.Errors.phone!=null||''||undefined)?this.validate.invalidPhone=true:this.validate.invalidPhone=false;
    (this.Errors.email!=null||''||undefined)?this.validate.invalidEmail=true:this.validate.invalidEmail=false;
    (this.Errors.query!=null||''||undefined)?this.validate.notFoundUser=true:this.validate.notFoundUser=false;

    },
    
    mounted(){
axios
.get("https://vue-study.skillbox.cc/api/users/accessKey")
.then(response=>this.order.accessKey=response.data.accessKey 
);
    }
}
</script>
<style lang="scss" scoped>

</style>