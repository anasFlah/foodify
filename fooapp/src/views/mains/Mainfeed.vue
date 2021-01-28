<template>
<div class="hero-inner1">

  <vs-nav
   :type="type"
    v-model="activeItem"
    
   class="navbar navbar-expand-lg navbar-mainbg">
   

  
    <a class="navbar-brand navbar-logo" href="#">Foodify</a>
    <button
      @click="test"
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <i class="fas fa-bars text-white"></i>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav ml-auto">
        <div class="hori-selector">
          <div class="left"></div>
          <div class="right"></div>
        </div>
        <span>
        <li class="nav-item">
          <a class="nav-link" href="javascript:void(0);"
            ><i class="fas fa-tachometer-alt"></i>{{name}}</a
          >
        </li>
        </span>
        <span>
        <li class="nav-item active">
          <a class="nav-link" href="javascript:void(0);"
            ><i class="far fa-address-book"></i>{{ userStatus }}</a
          >
        </li>
        </span>
        <span>
        <li class="nav-item active">
          <a class="nav-link" href="javascript:void(0);"
            ><i class="far fa-address-book" @click="adminDashboard"></i>Admin Dashboard</a
          >
        </li>
        </span>
 <li  class="nav-item active"  >
          <a class="nav-link" href="javascript:void(0);"
            ><i class="far fa-address-book" ></i><span class="far fa-address-book" @click="logout">logout</span></a
          >
         </li>
      </ul>
    </div>
   </vs-nav>

 
    <div v-if="allposts" id="columns">
      <div class="under-columns" v-for="post in allposts" :key="post._id">
        <figure @click="goToPost(post._id)">
            <img :src= post.image />
            <figcaption> {{post.title}} </figcaption>
        </figure>
      </div>
    </div>
   </div>
  
</template>

<script>
import $ from "jquery";
import axios from 'axios';
const Cookies = require("js-cookie");
// ---------Responsive-navbar-active-animation-----------

export default {
  name: "Mainfeed",
  data: () => ({
    name: Cookies.get("name"),
    userStatus: Cookies.get("status"),
    popupActivo: false,
    type: "type",
    activeItem: "activeItem",
    allposts: null
  }),
  async mounted(){
    const allpostsDum = await axios.get("/api/dummieposts");
    this.allposts = allpostsDum.data;
  },
  created() {
    var that = this;
    setTimeout(function() {
      that.test();
    });
    window.onresize = () => {
      setTimeout(function() {
        that.test();
      }, 500);
    };
  },
  methods: {
    logout() {
      Cookies.remove("name");
      Cookies.remove("_id");
      Cookies.remove("status");
      document.location.replace('/');
    },
    adminDashboard(){
      this.$router.push("/admindashboard");
    },
    goToPost(id){
      this.$router.push(`/post/${id}`)
    },
    test() {
      var tabsNewAnim = $("#navbarSupportedContent");
      // var selectorNewAnim = $("#navbarSupportedContent").find("li").length;
      var activeItemNewAnim = tabsNewAnim.find(".active");
      var activeWidthNewAnimHeight = activeItemNewAnim.innerHeight();
      var activeWidthNewAnimWidth = activeItemNewAnim.innerWidth();
      var itemPosNewAnimTop = activeItemNewAnim.position();
      var itemPosNewAnimLeft = activeItemNewAnim.position();
      $(".hori-selector").css({
        top: itemPosNewAnimTop.top + "px",
        left: itemPosNewAnimLeft.left + "px",
        height: activeWidthNewAnimHeight + "px",
        width: activeWidthNewAnimWidth + "px",
      }),
        $("#navbarSupportedContent").on("click", "li", function() {
          $("#navbarSupportedContent ul li").removeClass("active");
          $(this).addClass("active");
          var activeWidthNewAnimHeight = $(this).innerHeight();
          var activeWidthNewAnimWidth = $(this).innerWidth();
          var itemPosNewAnimTop = $(this).position();
          var itemPosNewAnimLeft = $(this).position();
          $(".hori-selector").css({
            top: itemPosNewAnimTop.top + "px",
            left: itemPosNewAnimLeft.left + "px",
            height: activeWidthNewAnimHeight + "px",
            width: activeWidthNewAnimWidth + "px",
          });
        });
    },
  },

  // $({window}).on('resize', =>(){
  //   setTimeout(function(){ test(); }, 500);
  // }),
};
</script>
<style scoped>

@import url("https://fonts.googleapis.com/css?family=Roboto");
.hero-inner1 {
  
  background-size: cover;
  background-position: center;
  display: table;
  width: 100%;
  height: 100vh;
  position: relative;
  top: 0;
}
span {
  cursor: pointer;
  
}
.spacing-navbar-element {
  margin: 0 0.6rem;
}
#columns {
  column-width: 320px;
  column-gap: 15px;
  width: 90%;
  max-width: 85%;
  margin: 3% auto;
}

div#columns figure {
  border-radius: 10px;
  background: #fefefe;
  border: 2px solid #fcfcfc;
  box-shadow: 0 1px 2px rgba(34, 25, 25, 0.4);
  margin: 0 2px 15px;
  padding: 15px;
  padding-bottom: 10px;
  transition: opacity 0.4s ease-in-out;
  display: inline-block;
  column-break-inside: avoid;
}

div#columns figure img {
  width: 100%;
  height: auto;
  border-bottom: 1px solid #ccc;
  padding-bottom: 15px;
  margin-bottom: 5px;
}

div#columns figure figcaption {
  font-size: 0.9rem;
  color: #444;
  line-height: 1.5;
}

div#columns small {
  font-size: 1rem;
  float: right;
  text-transform: uppercase;
  color: #aaa;
}

div#columns small a {
  color: #666;
  text-decoration: none;
  transition: 0.4s color;
}

figure:hover {
  cursor: pointer;
  transform: scale(1.04);
}

@media screen and (max-width: 750px) {
  #columns {
    column-gap: 0px;
  }
  #columns figure {
    width: 100%;
  }
}
body {
  font-family: "Roboto", sans-serif;
}
* {
  margin: 0;
  padding: 0;
}
i {
  margin-right: 10px;
}
/*----------multi-level-accordian-menu------------*/
.navbar-logo {
  padding: 15px;
  color: color;
}
.navbar-mainbg {
  background-color: black;
  padding: 0px;
}
#navbarSupportedContent {
  overflow: hidden;
  position: relative;
}
#navbarSupportedContent ul {
  padding: 0px;
  margin: 0px;
}
#navbarSupportedContent ul li a i {
  margin-right: 10px;
}
#navbarSupportedContent li {
  list-style-type: none;
  float: left;
}
#navbarSupportedContent ul li a {
  color: rgba(255, 255, 255, 0.5);
  text-decoration: none;
  font-size: 15px;
  display: block;
  padding: 20px 20px;
  transition-duration: 0.6s;
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55);
  position: relative;
}
#navbarSupportedContent > ul > li.active > a {
  color: orange;
  background-color: transparent;
  transition: all 0.7s;
}
#navbarSupportedContent a:not(:only-child):after {
  content: "\f105";
  position: absolute;
  right: 20px;
  top: 10px;
  font-size: 14px;
  font-family: "Font Awesome 5 Free";
  display: inline-block;
  padding-right: 3px;
  vertical-align: middle;
  font-weight: 900;
  transition: 0.5s;
}
#navbarSupportedContent .active > a:not(:only-child):after {
  transform: rotate(90deg);
}
.hori-selector {
  display: inline-block;
  position: absolute;
  height: 100%;
  top: 0px;
  left: 0px;
  transition-duration: 0.6s;
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55);
  background-color: #fff;
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  margin-top: 10px;
}
.hori-selector .right,
.hori-selector .left {
  position: absolute;
  width: 25px;
  height: 25px;
  background-color: #fff;
  bottom: 10px;
}
.hori-selector .right {
  right: -25px;
}
.hori-selector .left {
  left: -25px;
}
.hori-selector .right:before,
.hori-selector .left:before {
  content: "";
  position: absolute;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color:black;
}
.hori-selector .right:before {
  bottom: 0;
  right: -25px;
}
.hori-selector .left:before {
  bottom: 0;
  left: -25px;
}

@media (max-width: 991px) {
  #navbarSupportedContent ul li a {
    padding: 12px 30px;
  }
  .hori-selector {
    margin-top: 0px;
    margin-left: 10px;
    border-radius: 0;
    border-top-left-radius: 25px;
    border-bottom-left-radius: 25px;
  }
  .hori-selector .left,
  .hori-selector .right {
    right: 10px;
  }
  .hori-selector .left {
    top: -25px;
    left: auto;
  }
  .hori-selector .right {
    bottom: -25px;
  }
  .hori-selector .left:before {
    left: -25px;
    top: -25px;
  }
  .hori-selector .right:before {
    bottom: -25px;
    left: -25px;
  }
}
</style>
