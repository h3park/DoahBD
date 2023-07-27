<template>
  <div id="container">
    <!-- 메인 -->
    <div class="mainSection full">
      <!-- <div style="width:100%; height:100%">
        <img src="../assets/mainBack.jpg" class="image-fill">
      </div> -->
      <div style="width:100%; height:100%; position: relative;">
        <img src="../assets/mainBack6.png" class="image-fit">
      </div>

      <!-- <h1 class="paragraph temp-abs" style="top:0;">도아의 첫번째 생일</h1> -->
      <div class="temp-abs" style="height:23%; top:100px">
        <img src="../assets/words/mainTitle.png" class="image-contain">
      </div>
      <div class="temp-abs" style="height:60%; top: 23%">
        <img src="../assets/MAIN1.png" class="mainImg image-contain">
      </div>
      <div class="paragraph temp-abs" style="height: 17%; top:83%">
        <h4>2023.08.20 12시</h4>
        <h4>서울클럽
          <span style="font-size:1.4rem">지리산룸(ROOM)</span></h4>
      </div>
      <!-- <img src="../assets/doahJoah_100.jpg"  class="image-fit" z-index=0>
      <div class="imgBlur">
        <div style="padding: 2rem;">
          <p style="color:white; font-size: 1.5rem; opacity=1">도아의 돌잔치에<br>초대합니다</p>
        </div>        
      </div> -->
    </div>
    
    <!-- 초대장 -->
    <div class="invitationSection full" style="height:100%; display: block" >
      <div class="invitationCard">
        <img src="../assets/invitationTop.png" class="image-fill" style="height:5%;">
        <img src="../assets/words/invitation1.png" class="image-contain sub-title-image">
        <p class="paragraph card-content size-limit">예쁜 미소를 가진 도아가 
          건강하게 자라서 첫 돌을 맞이했습니다
          태어나 이 세상에 적응하고 있는 
          도아의 첫 생일을 
          함께 축하해 주셔서 감사합니다
        </p>
        <!-- 연락처 -->
        <div class="contactSection size-limit" style="width:65%">
          <div v-for="(telNum, p) in parentTel" :key="p" class="singleContact">
            <h4 style="display:flex; align-items:bottom; ">{{p}}에게 연락하기</h4>
            <div class="contactPhone">
              <button :href="'tel:'+telNum">
                <b-icon icon="telephone-fill" :style="[p==='아빠'? 'color: #89cff0;':'color:#f4c2c2;']"></b-icon></button>
              <span class="divider" :style=" p === '아빠' ? 'background-color:#89cff0': 'background-color: #f4c2c2'"></span>
              <button :href="'sms:'+telNum">
                <b-icon icon="envelope-fill" :style="[p==='아빠'? 'color: #89cff0':'color:#f4c2c2']"></b-icon>
              </button>
            </div>
          </div>
        </div>
        <!-- 달력? -->
        <div class="calendarSection">
          <b-calendar
            id="my_calendar"
            ref="calendar"
            disabled=true
            hide-header=true
            no-key-nav=true
            label-help=""
            value="2023-08-20"
            readonly=true
            selected-variant="info"
            block 
          >
          </b-calendar>
          <!-- 까지 며칠 남았습니다 -->
        </div>
      </div>
    </div>

    <!-- 사진 -->
    <div class="gallerySection full">
      <div></div>
      <img src="../assets/words/gallery.png" class="image-contain sub-title-image">
      <div style="height:calc(100% - 22vmin); padding-bottom:3rem;">
        <!-- <img :src="require('../assets/gallery/gallery'+currImgIndex+'.jpg')"
          class="main-img"
          @mouseover="Hover(true)" @mouseleave="Hover(false)"> -->
        <hooper group="group1" style="height: 85%; padding-bottom:1.5rem;" class="size-limit" :wheelControl="false" :infiniteScroll="true">
          <slide
            v-for="(_,index) in photoCnt" 
            :key="index" >
            <img                  
              :src="require('../assets/gallery/gallery'+index+'.jpg')"              
              class="main-img"
              @mouseover="Hover(true)" @mouseleave="Hover(false)"
            >              
          </slide>     
          <hooper-navigation slot="hooper-addons"></hooper-navigation>       
        </hooper>
        <hooper group="group1" ref="gallery" class="sub-photo-gallery size-limit"
          :itemsToShow="8" :centerMode="true" :infiniteScroll="true" :wheelControl="false"
          @slide="updateCarousel({$event})" 
          @updated="UPD" >
          <slide
            v-for="(_,index) in photoCnt" 
            :key="index" >
            <img                  
              :src="require('../assets/gallery/gallery'+index+'.jpg')"
              @click="ImageClicked($event,index)"
              class="sub-img">              
          </slide>            
        </hooper>
      </div>
    </div>

    <!-- 지도 -->
    <div class="dirSection full">
      <img src="../assets/words/location.png" class="image-contain sub-title-image">
      <div class="dirContent size-limit" style="width: 70%; max-width: 800px;">
        <div id="map"></div>  
        <!-- <b-button squared
          href="https://map.naver.com/v5/directions/-/14138002.419104088,4516506.529278252,%EC%84%9C%EC%9A%B8%ED%81%B4%EB%9F%BD,20202001,PLACE_POI/-/transit?c=15,0,0,0,dh">지도 열기</b-button> -->
        
        <b-button squared
          href="https://naver.me/GZAjsyPn">지도 열기</b-button>
        
        <div class="description">
          <div>
            <p><span style="font-size:1.4rem;font-weight:bold">{{placeName}}</span>{{placeNameDetail.length > 0 ? '  /  ':''}}{{placeNameDetail}}</p>
            <p>&ensp;<b-icon icon="geo-alt-fill" style="color: #7BC99D"></b-icon>&ensp;{{address}}</p>
            <p>&ensp;<b-icon icon="telephone" style="color: #7BC99D"></b-icon>&ensp;{{placeTel}}</p>
          </div>
          <!-- guide -->
          <div>
            <ul>
              <li v-for="(des, vehicle) in transport" :key="vehicle">
                <div>{{vehicle}}</div>
                <p class="paragraph">{{des}}</p>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <img src="../assets/locationBG.png" class="location-img">
    </div>
      

  </div>
  
</template>

<script>
import { Hooper, Slide,   Navigation as HooperNavigation } from 'hooper';
import 'hooper/dist/hooper.css';

export default {
  name: 'IndexPage',
  components: {
    Hooper,
    Slide,
    HooperNavigation
  },

  mounted(){    
    // 네이버 지도 추가
    const script = document.createElement('script');
    script.src = 'https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=i1bmdsyj4j';
    
    script.addEventListener("load", () =>{
      var map = new naver.maps.Map('map',{
        center: new naver.maps.LatLng(37.5532645, 127.0038206), //지도의 초기 중심 좌표
        zoom: 15, 
         minZoom: 7, //지도의 최소 줌 레벨
      })
      var marker = new naver.maps.Marker({
        position: new naver.maps.LatLng(37.5532645, 127.0038206),
        map: map
      })
    });
    
    document.head.appendChild(script);

    // console.log(Context.getPackageName());
    console.log(window.location.pathname)


    // 기기 알아보기
    let details = navigator.userAgent;

    let isApple = /iphone|kindle|ipad/i;
    let isAndroid = /android/i;
      
    /* Using test() method to search regexp in details
    it returns boolean value*/
    let isAppleDevice = isApple.test(details);
    let isAndroidDevice = isAndroid.test(details);

    console.log("========test==========")
    if (isAppleDevice || isAndroidDevice) {
        console.log("You are using a Apple Device");
    }
    // }else if(isAndroidDevice){
    //     console.log("You are using a Android Device");
    // } 
    else {
        console.log("You are using Desktop");
    }

  },
  data(){
    return{
      lat: 0,
      long: 0,
      mapURL: "nmap://actionPath?parameter=value&appname=http://www.localhost:3000",
      address: "서울 중구 장충단로 86",
      placeName: "서울클럽",
      placeNameDetail: "지리산룸",
      placeTel: "02-2238-7666",
      transport: {
        "교통편":"420 / 01 / 8001번 버스", 
        "자가용":"[ 서울클럽 게스트 주차장 ]\n\
          이용 시 차량등록을 진행하셔야\n 2시간 무료주차 가능합니다\n\
          [ 자유총연맹 주차장 ] \n\
          서울클럽 게스트 주차장에 위치한 초소에서 2시간 주차권을 발급 받아야 이용 가능합니다\n\
          2시간 초과시 주차 비용 발생"
      },
      parentTel:{"아빠":"01030365213", "엄마": "01028269110"},
      birthDay: "2023-08-23",
      calendarContext: null,
      photoCnt: 30,
      dragged: false,
      currImgIndex: 0,
      items: 7,
    }
  },
  methods:{
    onContext(ctx) {
      console.log(ctx)
            console.log("===")
    console.log(this.$refs.calendar)
    },
    ImageClicked(e,index){
      console.log(e)
      let slide = structuredClone(this.$refs.gallery.isSliding)
      console.log("clicked")
      console.log(this.$refs.gallery.isSliding)
      if(!slide) {
        this.currImgIndex = index;
        this.$refs.gallery.slideTo(index)
      }
      
      e.preventDefault()
    },
    BTNClicked(){
      $('.next').click(function() {
          owl.trigger('stop.owl.autoplay');
      })
    },
    UPD(){
      console.log("UPDATED")
    },
    updateCarousel(payload){
      console.log(this.$refs.gallery.isDragging)
      console.log("Update")
      console.log(payload)
      console.log(this.$refs.gallery)
      this.currImgIndex = payload.currentSlide%this.photoCnt
      this.dragged = true
    },
    Hover(val){
      this.$refs.gallery.isHover = val
      console.log(this.$refs.gallery.isHover)
    },
  }
}
</script>

<style>
body {
    min-width: 330px
}

#container {
  width: 100%;
  position: relative;
  outline: none;
  flex: 1 1 0%;
  /* background-color: #E3F4EA; */
  overflow-y: auto;
  font-family: 'Sunflower', sans-serif;
  /* font-family: 'Gowun Batang', serif; */
  /* padding-bottom: 7rem; */
}

body::-webkit-scrollbar{
    display: none;
    -webkit-appearance: none;
    overflow: hidden;
}

#container .size-limit{
  width: 86%;
  margin: auto;
  max-width: 900px;
  min-width: 300px;
}

#container h2{
  margin: 80px 0 30px 0;
  /* margin-top: 100px; */
  text-align: center;
  font-weight: bold;
}

.paragraph{
  white-space: pre-line;
}

.full{
  width: 100%;
  height:100vh;
  min-height: 600px;
}

/* 이미지 */
.image-fit{
  height: 100%;
  width: 100%;
  object-fit: cover;
}
.image-fill{
  height: 100%;
  width: 100%;
  object-fit: fill;
}
.image-contain{
  height: 100%;
  width: 100%;
  object-fit: contain;
  display: block;
  margin: auto;
  padding: 8px;
}
.sub-title-image{
  height: 25vmin;
  min-height: 100px;
  width: 35%;
  max-width: 500px;
  display: block;
  margin: 0 auto;
  padding: 5vmin 0;
}


/* Main */
.mainSection{
  background-color: white;
  position: relative;
}
/* 윗 글 */
.mainSection h1{
  margin: 0;
  height: 23%;
  display: flex;
  justify-content: center;
  align-items: end;
}
.mainSection .temp-abs{
  position: absolute;
  width:100%;
}
/* 이미지 */
.mainImg{
  height: 100%;
  display: block;
  margin: auto;
}
/* 아래 */
.mainSection{
  text-align: center;
}
.mainSection span{
  font-weight: 500;
}
/* .mainContainer{
  position: absolute;
  top: 0;
} */




.imgBlur{
  background-color:black; 
  height:100vh; 
  width:100vw; 
  position:absolute; 
  top:0;
  opacity: 0.2;
  z-index: 1;
}

/* 초대장 */
.invitationSection{
  position:relative;
}
.invitationCard{
  min-height: 100vh;
  height: 100%;
  width: 100%;
}
.invitationCard .card-content{
  line-height: 10vmin;
  font-size: 3vmin;
  text-align: center;
  margin: 5vmin auto !important;
}

/* 연락처 */
.contactSection{
  display: flex;
  flex-direction: row;
}
.contactSection .singleContact{
  display: flex;
  flex-direction: column;
  flex: 1;
  /* border: 3px #507a6b solid; */
  /* background-color: #E7F5ED; */
  /* background-color: #C7E9D6; */
  aspect-ratio: 1/0.5;
  width: calc(50% - 20px);
  height: 5%;
  min-height: 100px;
  margin: 10px;
}
.singleContact h4{
  font-size: 3.3vmin;
  height: 20%;
  align-items: end;
  justify-content: center;
  text-shadow: 2px 1px 2px gray;
}
.singleContact .divider{
  height: 36%;
  width: 0.2vw;
  margin: auto;
}
.contactPhone{
  display: flex;
  flex-direction: row;
  flex: 1;
}
.singleContact .contactPhone button{
  background-color: transparent; 
  border: none; 
  font-size:0.9rem;
  flex: 1;
}
.singleContact .contactPhone svg{
  width: 35%;
  max-width: 60px;
  height: 35%;
  max-height: 60px;
}

/* 갤러리 */
@media all and (orientation: landscape){
  .gallerySection{
    height: 100vh;
    min-height: 800px;
    background-color: #E7F5ED;
  }
  .gallerySection .sub-img{
    height: 100%;
    width: 100%;
    object-fit: cover;
    /* opacity: 0.7; */
    aspect-ratio: 1/1;
  }
}
@media all and (orientation: portrait){
  .gallerySection{
    height: 100vh;
    min-height: 600px;
    background-color: #E7F5ED;
  }
  .gallerySection .sub-img{
    width: 100%;
    object-fit: cover;
    /* opacity: 0.7; */
    aspect-ratio: 1/1;
  }
}

.gallerySection .main-img{
  height:100%; 
  width:100%;
  display: block;
  margin: auto;
  object-fit: contain;
}
.gallerySection .sub-photo-gallery{
  height: 15%;
}
.gallerySection .hooper-slide{
  padding: 0 2px;
  opacity: 0.7;
}
.gallerySection .is-current{
  /* border: 2px blue solid !important; */
  opacity: 1 !important;
  padding:0 2px;
}
.gallerySection .sub-photo-gallery .is-current img{
  outline: 3px #ffd667 solid; outline-offset:-3px
}


/* 달력 */
@media all and (orientation: landscape){
  #my_calendar__calendar-grid_{
    max-width: 750px;
    width:50%;
    margin: auto;
  }
}
@media all and (orientation: portrait){
  #my_calendar__calendar-grid_{
    max-width: 750px;
    width:80%;
    min-width: 300px;
    margin: auto;
  }
}
.calendarSection{
  padding-bottom: 5vmin;
}
#my_calendar__calendar-nav_{
  position: absolute;
  height: 1px;
  overflow: hidden;
}

#my_calendar__calendar-grid_ .text-muted{
  color: black !important;
}
#my_calendar__calendar-grid_ .text-dark{
  color: black !important;
}
#my_calendar__calendar-grid_ .b-calendar-grid-body .btn-info{
  opacity: 1 !important;
  background-color: #3CB39B;
}

.b-calendar-grid-body .no-gutters > .col, .no-gutters > [class*="col-"]{
  aspect-ratio: 1 / 0.9;
}

/* 지도 */
.dirSection .dirContent{
  width:70%;
  max-width: 800px;
  margin: auto;
  min-width: 300px;
}
.dirSection .dirContent #map{
  /* min-width:300px;  */
  aspect-ratio: 1 / 0.7; 
  min-height:100px;
  margin: auto
}
.dirSection .dirContent a{
  display: block;
  margin: auto;
  font-size: 2.8vmin;
  background-color: #9fd8b8;
  border-color: #9fd8b8;
  /* min-width: 300px */
}
.dirSection .description{
  margin: 5vmin 0;
  /* background-color: rgba(255,255,255, .75); */
  background-color: rgba(255,255,255, .75);
  border-radius: 10px;
}
.dirSection .description > div{
  margin-bottom: 1rem;
}
.dirSection .description p{
  margin: 0 0 0.3rem 0;
  word-break: keep-all;
}
.dirSection .description li{
  margin-bottom: 0.8rem;
}
.dirSection .description li > div{ 
  margin-bottom: 0.2rem;
  font-size: 1.2rem;
  font-weight: 500;
}
.dirSection .description .place-name{
  font-weight: bold;
}
@media all and (orientation: landscape){
  .dirSection .location-img{
    object-fit: fill;
    width: 100%;
    z-index: -1;
  }
}
@media all and (orientation: portrait){
  .dirSection .location-img{
    object-fit: fill;
    position:absolute;
    bottom:0;
    width: 100%;
    z-index: -1;
  }
}

</style>>

