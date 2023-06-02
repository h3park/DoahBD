<template>
  <div id="container">
    <!-- 메인 -->
    <div class="mainSection ">
      <img src="../assets/doahJoah_100.jpg"  class="image-fit" z-index=0>
      <div class="imgBlur">
        <div style="padding: 2rem;">
          <p style="color:white; font-size: 1.5rem; opacity=1">도아의 돌잔치에<br>초대합니다</p>
        </div>
        
      </div>
    </div>
    <!-- 사진 -->
    <div style="height:100vh">
      <h3>VIEW PHOTOS</h3>
      <div style="height:90%;">
        <img :src="require('../assets/gallery/gallery'+currImgIndex+'.jpg')"
          class="main-img"
          v-on:mouseover="Hover"
          >
        <carousel ref="photoList" id="photoList"
          :items="10"
          :margin="6"
          :freedrag="true" 
          :nav="false" 
          :loop="true"
          :center="true"
          :dots="false"
          :auto-height="false"
          :rewind="false"
          :startPosition="0"
          :autoplay="autoplayFlag"
          :autoplayTimeout="autoplayTime"
          :autoplayHoverPause="true"
          @changed="changed"
          @update="updated"
        >
          <img v-for="(_,index) in photoCnt" 
            :key="index" 
            :src="require('../assets/gallery/gallery'+index+'.jpg')"
            @click="GoTo(index)"
            class="image-fit">
        </carousel>
      </div>
      <!-- 하단 -->
      

      <div>

      </div>
    </div>

    <!-- 인사? -->
    <div>
      <p>{{}}</p>
    </div>

    <!-- 달력? -->
    <div>
      <b-calendar
        id="my_calendar"
        ref="calendar"
        disabled=true
        hide-header=true
        no-key-nav=true
        label-help=""
        value="2023-08-23"
        readonly=true
        selected-variant="info"
        block 
      >
      </b-calendar>
      <!-- 까지 며칠 남았습니다 -->
    </div>

    <!-- 지도 -->
    <div class="dirSection">
      <p>위치</p>
      <div class="mapUI">
        <div id="map"></div>  
        <b-button squared
          style="width:100%"
          href="https://map.naver.com/v5/directions/-/14138002.419104088,4516506.529278252,%EC%84%9C%EC%9A%B8%ED%81%B4%EB%9F%BD,20202001,PLACE_POI/-/transit?c=15,0,0,0,dh">지도 열기</b-button>
      </div>
      <div>
        <p style="font-weight: bold">{{placeName}}<span style="font-size:0.9rem; font-weight: normal">{{placeNameDetail.length > 0 ? '/':''}}{{placeNameDetail}}</span></p>
        <p>{{address}}</p>
        <span>{{placeTel}}</span>

        <!-- guide -->
        <div>
          <ul>
            <li v-for="(des, vehicle) in transport" :key="vehicle">
              <div>{{vehicle}}</div>
              <div style="white-space: pre-line">{{des.join('\n')}}</div>
            </li>
          </ul>
        </div>
      </div>

       

    </div>
      
    <!-- 연락처 -->
    <div class="contactSection">
      <div v-for="(telNum, p) in parentTel" :key="p" class="singleContact">
        <p>{{p}}에게 연락하기</p>
        <div class="contactPhone">
          <button :href="'tel:'+telNum">
            <b-icon icon="telephone-fill" :style="[p==='아빠'? 'color: #89cff0;':'color:#f4c2c2;']"></b-icon></button>
          <button :href="'sms:'+telNum">
            <b-icon icon="envelope-fill" :style="[p==='아빠'? 'color: #89cff0':'color:#f4c2c2']"></b-icon>
          </button>
        </div>
      </div>
    </div>
    <!-- 공유하기? -->
  </div>
</template>

<script>
import carousel from 'vue-owl-carousel'

export default {
  name: 'IndexPage',
  components: {
    carousel,
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
    if (isAppleDevice) {
        console.log("You are using a Apple Device");
    }else if(isAndroidDevice){
        console.log("You are using a Android Device");
    } 
    else {
        console.log("You are using Desktop");
    }

    console.log("===")
    // console.log(this.$refs.calendar)
    // this.$refs.calendar.activeYMD = "2023-08-23"
    // this.$refs.calendar.selectedDate = "2023-08-23"
  },
  data(){
    return{
      lat: 0,
      long: 0,
      mapURL: "nmap://actionPath?parameter=value&appname=http://www.localhost:3000",
      address: "서울 중구 장충단로 86",
      placeName: "서울클럽",
      placeNameDetail: "",
      placeTel: "02-2238-7666",
      transport: {"지하철":["동대문","3번 출구"], "버스":[], "승용차":[]},
      parentTel:{"아빠":"01030365213", "엄마": "01028269110"},
      birthDay: "2023-08-23",
      calendarContext: null,
      slide: 0,
      sliding: null,
      photoCnt: 30,
      dragged: false,
      currImgIndex: 0,
      autoplayFlag: true,
      autoplayTime: 1000,
    }
  },
  methods:{
    onContext(ctx) {
      console.log(ctx)
            console.log("===")
    console.log(this.$refs.calendar)
    },
    changed(){
      // this.dragged = true;
      console.log('changed')
      //       return

      // console.log(this.$refs.photoList)
    },
    updated(){
      console.log('updated')
      console.log(this.$refs.photoList)
    },
    ImageClicked(index){
    },
    Hover(){
            console.log(this.$refs.photoList)

      this.autoplayFlag = false
      this.$refs.photoList.autoplay = false
  console.log("==============")
console.log(this.$refs.photoList.$props["get autoplay"])
      
      console.log(this.$refs.photoList.$data.showNext)
      // this.$refs.photoList_props.autoplay(false);
      // this.$refs.photoList.$porps.autoplay = false

      
    }
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
  background-color: #ffe5eb;
  overflow-y: auto;
  font-family: 'Sunflower', sans-serif;
}
#container::-webkit-scrollbar {
  display: none;
}
#container .mainSection{
  width: 100%;
}
#container > div{
  width: 86%;
  margin: auto;
  display: block;
}
#container h3{
  /* margin-top: 100px; */
  text-align: center;
  font-weight: bold;
}
.full{
  width: 100vw;
  height: 100vh;
}
.mainSection{
  width: 100vw;
  height:100vh;
  background-color: antiquewhite;
}
.image-fit{
  height: 100%;
  width: 100%;
  object-fit: cover;
}
.cover{
  object-fit: cover;
}

.imgBlur{
  background-color:black; 
  height:100vh; 
  width:100vw; 
  position:absolute; 
  top:0;
  opacity: 0.2;
  z-index: 1;
}


/* 갤러리 */
/* #photoList div{
  min-height: 150px;
  max-height: 250px;
} */
#photoList{
  height: 20%;
}
.main-img{
  height:80%; 
  min-height: 500px;
  display: block;
  margin: auto;
  padding-bottom: 1rem;
}
#photoList .owl-stage > div{
  aspect-ratio: 1/1;
}

/* 달력 */
#my_calendar__calendar-nav_{
  position: absolute;
  height: 1px;
  overflow: hidden;
}
#my_calendar__calendar-grid_{
  max-width: 650px;
  margin: auto;
}
#my_calendar__calendar-grid_ .text-muted{
  color: black !important;
}
#my_calendar__calendar-grid_ .text-dark{
  color: black !important;
}
#my_calendar__calendar-grid_ .b-calendar-grid-body .btn-info{
  opacity: 1 !important;
  background-color: #fc5a8d;
}

.b-calendar-grid-body .no-gutters > .col, .no-gutters > [class*="col-"]{
  aspect-ratio: 1 / 0.9;
}

/* 지도 */
.dirSection .mapUI #map{
  max-width:650px; 
  width: 100%; 
  /* min-width:300px;  */
  aspect-ratio: 1 / 0.7; 
  min-height:300px;
  margin: auto
}
.dirSection .mapUI a{
  display: block;
  margin: auto;
  max-width: 650px;
  /* min-width: 300px */
}
/* 연락처 */
.contactSection{
  display: flex;
  flex-direction: row;;
}
.singleContact{
  display: flex;
  flex-direction: column;
  flex: 1;
  border: 3px solid;
  width:50%;
}
.contactPhone{
  display: flex;
  flex-direction: row;
  flex: 1;
}
.singleContact .contactPhone button{
  background-color: transparent; 
  border: none; 
  font-size:0.9rem
}
.singleContact .contactPhone svg{
  width: 30px;
  height: 30px;
}

</style>>

