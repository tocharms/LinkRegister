
<template>
  <div>
    <v-app-bar
      color="primary"
      dense
      flat
      dark
    >
      <v-toolbar-title>Register</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
         <v-col cols="12" class="text-center pb-0 profile-img">
        <img v-if="getLine.pictureUrl == ''" src="~/assets/profile.png" alt="" width="155">
        <img v-else :src="getLine.pictureUrl" alt="" width="155">
        </v-col>
      <v-col cols="12" class="text-center pt-2 pb-0">
         Displayname
        </v-col>
        <v-col cols="12">
          <v-form>
            <!--p class="text-center text-main mb-0 mt-4">Displayname</-->
              <v-text-field
                  v-model="form.lineid"
                  label="Line ID"
                ></v-text-field>
            <v-text-field
              v-model="form.phone"
              dense          
              :rules="phoneRules"
              @keypress="onlyNumber($event, 10)"
              label="Phone"
            ></v-text-field>
             <v-text-field
              v-model="form.email"
              type="email"
              dense      
              :rules="emailRules"
              label="Email"
            ></v-text-field>           
            <p class="text-center text-main mb-0 mt-4">Work Profile</p>
            <v-text-field
              v-model="form.circuitid1"
              dense          
              label="Circuit Id1"
            ></v-text-field>
            <v-text-field
              v-model="form.circuitid2"
              dense          
              label="Circuit Id2"
            ></v-text-field>
            <v-btn rounded color="primary" dark class="w-100 mt-10 my-btn" @click="register">Register</v-btn>
            <div class="w-100 text-center my-btn text-primary" @click="back">Close</div>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
const REGEX_EMAIL = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
const REGEX_PHONE = /^[0]([0-9]{9})*$/
const REGEX_NUMBER = /^[0-9]*$/


import axios from 'axios'
import { constants } from 'buffer'



export default {
   mounted(){
    liff.init({
      liffId: '1657128604-LyreQngl'
    }).then(() => {
      if(liff.isLoggedIn()){
        liff.getProfile().then(profile => {                    
          this.$store.dispatch('setLine', profile);
          this.isDone();
        })
      }else{
        liff.login();
      }
    })
  }, 
  computed: {
    getLine(){
      return this.$store.getters.getLine;      
    }
  }, 
  data(){
    return {
      form: {
        lineid: this.$store.getters.getRegister.LINEID,
        phone: this.$store.getters.getRegister.PHONE,
        email: this.$store.getters.getRegister.EMAIL,
        circuitid1: this.$store.getters.getRegister.CIRCUITID1,
        circuitid2: this.$store.getters.getRegister.CIRCUITID2
      },
      modal: false,
      emailValidated: false,
      phoneValidated: false,
       emailRules: [ value => this.emailValidator(value)],
       phoneRules: [ value => this.phoneValidator(value)]
    }
  },
  methods: {
    phoneValidator(value){    
      this.phoneValidated = false  
      if(value == ''){
        return 'required'
      }
      if(REGEX_PHONE.test(value) && value.length == 10){ 
        this.phoneValidated = true
        return true
      }
      return "please input phone number"
    },
    emailValidator(value){
      this.emailValidated = false
      if(value == ''){
        return 'required'
      }
      if(REGEX_EMAIL.test(value)){
        this.emailValidated = true
        return true
      }
      return "email is Invalid"
    },
    onlyNumber(event, max){  
      if(event.target.value.length == 0){
        if(event.key != 0){
          return event.preventDefault()
        }
      }else{
        if(!REGEX_NUMBER.test(event.key) || event.target.value.length == max){
          return event.preventDefault()
        }
      }      
    }, 
    validate(){
      let validated = true
      const errors = []
      const validatorField = [
        'email',
        'phone',
        'circuit1'
      ]
      validatorField.forEach((field) => {
        if(this.form[field] == ''){
          validated = false
          errors.push(`${field} can not be null`)
        }
      })    
      if(!this.emailValidated){
        validated = false
        errors.push(`email is Invalid`)
      }
      if(!this.phoneValidated){
        validated = false
        errors.push(`please input phone number`)
      }
      if(!validated){
        this.$store.dispatch('setDialog', {
          isShow: true,
          title: 'Form is error',
          message: errors.map((error) => error+'<br/>').join('')
        })        
      }      
      return validated
    },  
    back() {      
      this.$router.push('/register')
    },
    register() {
      if(this.validate()){
       this.$store.dispatch('setRegister', this.form)
        this.$axios.post("https://localhost:5001/api/LiffRegisterApi", this.$store.getters.getRegister).then((res) => {
        this.$router.push('/register/done')
      }).catch(e => console.log(e))         
    // alert(this.$store.getters.getRegister);
//this.formSubmit()
      }      
    },
  formSubmit() {
  const url = 'https://localhost:44336/Api/LiffRegister'
     axios.post(url, {
  "LINEUSERID": "LINEUSERID1",
  "LINEID": "LINEID2",
  "PHONE": "PHONE3",
  "EMAIL": "EMAIL4@yahoo.com",
  "CIRCUITID1": "CIRCUITID1",
  "CIRCUITID2": "CIRCUITID2",
  "REGISTERDATE": "2022-06-16T01:38:53.3273359+07:00"
}).then((res) => {
         this.$router.push('/register/done')
       }).catch(e => console.log(e)) 
    }
  
  }
  
}



</script>

<style lang="scss" scoped>
  .v-form{
    padding: 0 10px;
  }
  .set-birthday{
    position: relative;
    ::v-deep .v-input__prepend-outer{
      position: absolute;
      right: 0
    }
  }
</style>