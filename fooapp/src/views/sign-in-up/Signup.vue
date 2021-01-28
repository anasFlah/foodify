<template lang="html">
  <div id="pop-btn">
 
    <vs-button
      @click="popupActivo = true"
      color="primary"
      type="filled"
      class="button-nav-spacing"
      >Sign Up</vs-button
    >
    <vs-popup
      class="signup-form signup-form signup-form"
      title="Create Your Account !"
      :active.sync="popupActivo"
    >
     
      <vs-input
        label="Username"
        class="horizontal-group form-group left form-group right label-title"
        placeholder="Placeholder"
        v-model="fname"
      />
      <vs-input
        label="email"
        class="inputx"
        placeholder="email"
        v-model="email"
      />
      <vs-input
        type="password"
        label="Password"
        class="horizontal-group form-group left label-title form-input"
        placeholder="Password"
        v-model="password"
      />
        <div class="form-group right">
    <label for="confirm-password" class="label-title">Confirm Password *</label>
    <vs-input  v-model="password1" type="password" class="form-input" id="confirm-password" placeholder="enter your password again" required="required"></vs-input>
  </div>
            <div class="horizontal-group">
  
  <div class="form-group right">
    <label for="experience" class="label-title">Age</label>
    <vs-slider :color="colorx" v-model="value1"/>
    
  </div>
 </div> 
      <span
        @click="
          checkemptyinput(),
            signup(fname, email, password),
            $vs.notify({
              title: 'Support',
              text: 'thank you for joining our community',
              color: 'primary',
            })
        "
      >
        <vs-button
          @click="openLoadingColor"
          type="filled"
          color="primary"
          class="form-header"
          >Register</vs-button
        >
      </span>
  
    </vs-popup>
  
  </div>
</template>

<script>
let bcrypt = require("bcryptjs");
const axios = require("axios");
export default {
  /* eslint-disable */

  name: "Signup",
  data() {
    return {
      colorx:"primary",
       value1:24,
      colorLoading: "#7d0c3f",
      popupActivo: false,
      fname: "",
      email: "",
      password: "",
      password1:"",
    };
  },
  methods: {
    async signup(username, useremail, password,password1) {
      var salt = bcrypt.genSaltSync(10);
      var hash = bcrypt.hashSync(password,password1, salt);
      try {
        const resp = await axios.post(
          "http://localhost:3000/api/loginsignup/signup",
          { username, useremail, password: hash,password1:hash }
        );
        this.popupActivo = false;
        console.log("saved");
      } catch (err) {
        // Handle Error Here
        console.error(err);
      }
      
    },
    //////////////////////////////////////////////////
    checkemptyinput() {
      if (
        this.fname.length < 1 ||
        this.email.length < 1 ||
        this.email.length < 1
      ) {
        alert("fill the form pls");
        return;
      }
    },
    //////////////////////////////////////////////////
    openLoadingColor() {
      this.$vs.loading({ color: this.colorLoading });
      setTimeout(() => {
        this.$vs.loading.close();
      }, 2000);
    },
    randomIcon() {
      function getRandomInt(min, max) {
        return Math.floor(Math.random() * (max - min)) + min;
      }
      let color = `rgb(${getRandomInt(0, 255)},${getRandomInt(
        0,
        255
      )},${getRandomInt(0, 255)})`;
      this.$vs.notify({
        title: "Icon mail",
        text: "Lorem ipsum dolor sit amet, consectetur",
        color: color,
        icon: "verified_user",
      });
    },
  },
};
   var rangeLabel = document.getElementById("range-label");
      var experience = document.getElementById("experience");
      function change() {
      rangeLabel.innerText = experience.value + "K";
      }
</script>
<style scoped>
@import url('httpss://fonts.googleapis.com/css?family=Roboto');

.avataro{

}

.signup-form {
  font-family: "Roboto", sans-serif;
  width:600px auto;
  margin:65px auto;
  border-radius: 100px;
}
.form-header h1 {
  font-size: 30px;
  text-align:center;
  color:#666;
  padding:20px 0;
  border-bottom:1px solid #cccccc;
}
.button-nav-spacing {
  width: 7.6rem;
}
.button-inside-sign-spacing {
  width: 7.6rem;
  margin: 1.2rem 0 1rem 0;
}
</style>
