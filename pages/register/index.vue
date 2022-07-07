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
        <v-col cols="12">          
          <div class="mt-8 text-primary text-title text-center">
            Step 1 of 2
          </div>
        </v-col>
        <v-col cols="12" class="text-center pb-0 profile-img">
        <img src="~/assets/profile.png" alt="" width="155">
        </v-col>
      <v-col cols="12" class="text-center pt-2 pb-0">
         Displayname
        </v-col>
         <v-col cols="12">
           <v-form>
            <v-text-field
              v-model="form.LineID"
              dense          
              label="LineID"
            ></v-text-field>
               <v-text-field
              v-model="form.PhoneNo"
              dense         
              label="Phone No"
            ></v-text-field>
             <v-text-field
              v-model="form.Email"
              dense         
              label="Email"
            ></v-text-field>

              <v-btn rounded color="primary" dark class="w-100 mt-10 my-btn" @click="next">Register</v-btn>
           </v-form>
         </v-col>
      </v-row>
    </v-container>        
   
  </div>
</template>

<style lang="scss" scoped>
  .v-form{
    padding: 0 10px;
  }
  .profile-img{
    img{
      border-radius: 50%;
    }
  }
  .gender-group{
    p{
      margin-bottom: 0;
      align-self: center;
      margin-right: 20px;
    }
    .circle{
      width: 45px;
      height: 45px;      
      color: #FFF;
      border-radius: 50%;
      position: relative;
      background: rgba($color: #000000, $alpha: 0.3);
      margin-right: 7px;
      &.active{
        background: #1A56BE;
      }
      svg{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    }
    

  }
  .w-100{
    width: 100%;
  }
  .my-btn{
    font-size: 20px !important;
    text-transform: none !important;
    height: auto !important;
    padding: 10px 0 !important;
    font-weight: bold;
  }
</style>

<script>
export default { 
  data(){    
    return {   
      form: {
        LineID: '',
        PhoneNo: '',
        Email: ''   
      }   
    } 
  },
   methods: {   
      validate(){
      let validated = true
      const errors = []
      let errorMsg = ''
      const validatorField = [
        'LineID',
        'Email'
      ]
      validatorField.forEach((field) => {
        if(this.form[field] == ''){
          validated = false
          errors.push(`${field} can not be null`)
        }
      })
      if(!validated){
         //  this.errorMsg =  errors.map((error) => error+'<br/>').join('')
         //  this.dialog=true
          this.$store.dispatch('setDialog', {
          isShow: true,
          title: 'Form is error',
          message: errors.map((error) => error+'<br/>').join('')
        })        
      }
     // console.log(errorMsg);
      return validated
       },
     next(){      
      if(this.validate()){
      this.$store.dispatch('setRegister', this.form)
       alert('OK');
      }      
     }
     
   }


}
</script>