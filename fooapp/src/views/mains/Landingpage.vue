<template>
  <header class="gridx">
    <div v-if="!this.loggedin">
      <vs-navbar
        :type="type"
        v-model="activeItem"
        color="black"
        class="topnavbar"
        text-color="orange"
        active-text-color="orange"
      >
   
        <!---
      Template logo
      -->
        <div slot="title" class="themelogo">
          <vs-avatar
            size="55px"
            src="https://i.ibb.co/WVnj7xk/2723432.png"
            alt="Dashboard"
          />
          <span class="logo-text"></span>
          <h3>
            foodify
          </h3>
        </div>
        <!---
      Mobile toggle
      -->
        <div slot="title">
          <div class="hiddenDesktop cursor-pointer" @click.stop="activeSidebar">
            <vs-icon icon="menu"></vs-icon>
          </div>
        </div>

        <vs-input
          size="50%"
          class="searching-bt"
          icon-after="true"
          icon="search"
          placeholder="Search Here"
          v-model="search"
        />

        <vs-spacer></vs-spacer>

        <!---
      Craete new dd
      -->
        <Signup />

        <Signin :login="this.login" />

        <!---
      Craete new dd
      -->
        <div class="d-flex align-items-center">
          <div class="d-flex align-items-center dropdownbtn-alignment">
            <vs-dropdown>
              <vs-button class="btn-drop" type="filled" icon="expand_more"
                >about</vs-button
              >
              <!-- <a href="#">Hola mundo</a> -->
              <vs-dropdown-menu>
                <vs-dropdown-item
                  label="Dashboard"
                  icon="dashboard"
                  @click="gotoAbout"
                  >About-Us</vs-dropdown-item
                >
              </vs-dropdown-menu>
            </vs-dropdown>
          </div>
        </div>
      </vs-navbar>
</div>
<div v-else>
      <Mainfeed />
    </div>
      <div class="hero">
        <div class="hero-inner">
          <div class="hero-title">
            <h1 class="text-light title font-2">Take a Deep Bite</h1>
          </div>
        
          <i class="chevron bottom"></i>
          <div class="col-md-7">
            <p style="padding-top: 47px"></p>
          </div>
        </div>
      </div>

     
    
      

    
  </header>
</template>

<script>
/* eslint-disable */

import Signin from "../sign-in-up/Signin";
import Signup from "../sign-in-up/Signup";
import Mainfeed from "../mains/Mainfeed";
const axios = require("axios");
const Cookies = require("js-cookie");
export default {
  components: {
    Signin,
    Signup,
    Mainfeed,
  },
  data: () => ({
    aos: "animation_name",
    loggedin: false,
    activeItem: 0,
    type: "Default",
    types: [
      {
        value: null,
        text: "Default",
      },
    ],
    colorx: "black",
    indexActive: 0,
    name: "Navbar",
    props: {
      topbarColor: {
        type: String,
        default: "black",
      },
      title: {
        type: String,
      },
      logo: {
        type: String,
      },
    },
    indexActive: 0,
    showToggle: false,
    search: "",
  }),
  mounted() {
    Cookies.get("name") ? (this.loggedin = true) : (this.loggedin = false);
  },
  methods: {
    ///////////////////////////////////////////////////
    activeSidebar() {
      this.$store.commit("IS_SIDEBAR_ACTIVE", true);
    },
    ////////////////////////////////////////////////////
    async login(useremail, password) {
      try {
        const resp = await axios.post("/api/loginsignup/login", {
          useremail,
          password,
        });
        if (resp.data === "user_not_found") {
          console.log("moush mawjoud");
          alert("wrong credentials");
        } else {
          Cookies.set("name", resp.data.username);
          Cookies.set("_id", resp.data._id);
          Cookies.set("status", resp.data.status);
          console.log(resp.data.status);

          document.location.reload(false);
        }
      } catch (err) {
        // Handle Error Here
        console.error(err);
      }
    },
    ///////////////////////////////////////////////////////
    gotoAbout() {
      this.$router.push("/about");
    },
  },
};
</script>

<style scoped>
@import url(
  https://fonts.googleapis.com/css?family=Montserrat:400,
  700|Josefin+Sans:400,
  600,
  700,
  400italic,
  600italic
);
#container {
  background-image: url("https://images.unsplash.com/photo-1505935428862-770b6f24f629");
}

.title {
  text-shadow: -0.1px -0.5px 0.5rem #000;
}

.topnavbar {
  opacity: 0.8;
  shadow: 1.6;
}
body {
  font-family: "josefin sans";
  font-size: 16px;
}
/* https://images.pexels.com/photos/704971/pexels-photo-704971.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500 */
.hero {
  height: 100vh;
  width: 100%;
  position: relative;
  top: 0;
  z-index: 9;
  transition: all 1.6s cubic-bezier(0.86, 0, 0.07, 1);
}
.hero.scrolled {
  transform: translate3d(0, -100%, 0) scale(0.75);
  opacity: 0;
}
.hero-inner {
  background-image: url("https://images.unsplash.com/photo-1505935428862-770b6f24f629");
  background-size: cover;
  background-position: center;
  display: table;
  width: 100%;
  height: 100vh;
  position: relative;
  top: 0;
}
.hero-title {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: montserrat;
}
.font-2 {
  font-family: "josefin sans";
  font-weight: 700;
}
.title {
  letter-spacing: 0.3em;
  text-transform: uppercase;
}
.text-light {
  color: #fff;
}
.font-alt {
  font-family: "georgia";
  font-style: italic;
  color: #666;
}
.hero {
  overflow: hidden;
  z-index: 1;
}
.content {
  background-image: url("https://images.unsplash.com/photo-1561284081-ebf6c977bbde");
  position: relative;
  background-color: #fff;
  border-top: 10px solid black;
  padding: 0;
  margin: 0;
  transition: all 1.6s cubic-bezier(0.86, 0, 0.07, 1);
  transform: translate3d(0, 20px, 0) scale(0.75);
  opacity: 0;
}
.content.scrolled {
  background-image: url("https://images.unsplash.com/photo-1561284081-ebf6c977bbde");
  transform: translate3d(0, 0, 0) scale(1);
  opacity: 1;
}
.sd {
  color: #fff;
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}
.sd:hover,
.sd:focus {
  color: #fff;
  opacity: 0.7;
  text-decoration: none;
}

@import url(https://fonts.googleapis.com/css?family=Roboto:400, 500, 300, 700);

* {
  font-family: Roboto;
}
.footyx {
  color: black;
}

@import url("https://fonts.googleapis.com/css2?family=Rubik:ital,wght@1,900&display=swap");
.themelogo {
  margin: 0 0.6rem;
}

.searching-bt {
  background-color: transparent;
  border: none;
  color: #333;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen-Sans, Ubuntu, Cantarell, "Fira Sans", "Droid Sans", "Helvetica Neue",
    Helvetica, "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", メイリオ,
    Meiryo, "ＭＳ Ｐゴシック", Arial, sans-serif, "Apple Color Emoji",
    "Segoe UI Emoji", "Segoe UI Symbol";
  font-size: 16px;
  font-weight: normal;
  height: 100%;
  outline: none;
  padding: 0;
  width: 40% !important;
}
h3 {
  font-family: "Rubik", sans-serif !important;
}
.btn-drop {
  margin-left: 14px;
  display: flow-root;
  margin-right: 12px;
}
/*////////////slide///////////////////////////////////////////////////////////////////////////////////////////////*/
.hero__scroll .chevron {
  margin-top: 20px;
  display: block;
  -webkit-animation: pulse 2s infinite;
  animation: pulse 2s infinite;
  color: #ff4081;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
user agent stylesheet i {
  font-style: italic;
}
.hero__scroll {
  position: absolute;
  bottom: 60px;
  width: 200px;
  margin: auto;
  display: block;
  cursor: pointer;
  padding-bottom: 40px;
  left: 0;
  right: 0;
  text-transform: uppercase;
}
.hero {
  width: 100%;
  height: 100vh;
  position: relative;
  overflow: hidden;
  color: #fff;
  text-align: center;
}
body {
  font-family: Helvetica, Tahoma;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
.chevron.bottom:before {
  top: 0;
  -webkit-transform: rotate(135deg);
  -ms-transform: rotate(135deg);
  transform: rotate(135deg);
}
.chevron::before {
  border-style: solid;
  border-width: 0.25em 0.25em 0 0;
  content: "";
  display: inline-block;
  height: 20px;
  position: relative;
  -webkit-transform: rotate(-45deg);
  -ms-transform: rotate(-45deg);
  transform: rotate(-45deg);
  vertical-align: top;
  width: 20px;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
0% {
  -webkit-transform: translate(0, 0);
  transform: translate(0, 0);
}
50% {
  -webkit-transform: translate(0, 10px);
  transform: translate(0, 10px);
}
100% {
  -webkit-transform: translate(0, 0);
  transform: translate(0, 0);
}
[data-aos][data-aos][data-aos-delay="800"].aos-animate,
body[data-aos-delay="800"] [data-aos].aos-animate {
  transition-delay: 0.8s;
}
[data-aos^="fade"][data-aos^="fade"].aos-animate {
  opacity: 1;
  transform: translateZ(0);
}
[data-aos][data-aos][data-aos-easing="ease"],
body[data-aos-easing="ease"] [data-aos] {
  transition-timing-function: ease;
}
[data-aos][data-aos][data-aos-delay="800"],
body[data-aos-delay="800"] [data-aos] {
  transition-delay: 0;
}
[data-aos][data-aos][data-aos-easing="ease-out-back"],
body[data-aos-easing="ease-out-back"] [data-aos] {
  transition-timing-function: cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
[data-aos][data-aos][data-aos-duration="1000"],
body[data-aos-duration="1000"] [data-aos] {
  transition-duration: 1s;
}
[data-aos^="fade"][data-aos^="fade"] {
  opacity: 0;
  transition-property: opacity, transform;
}
.hero__scroll {
  position: absolute;
  bottom: 60px;
  width: 200px;
  margin: auto;
  display: block;
  cursor: pointer;
  padding-bottom: 40px;
  left: 0;
  right: 0;
  text-transform: uppercase;
}
[data-aos="fade-up"] {
  transform: translate3d(0, 100px, 0);
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
.hero {
  width: 100%;
  height: 100vh;
  position: relative;
  overflow: hidden;
  color: #fff;
  text-align: center;
}
body {
  font-family: Helvetica, Tahoma;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
</style>
