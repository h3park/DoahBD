<template>
  <div id="container">
    <!-- 메인 -->
    <div class="mainSection">
      <img src="../assets/doahJoah_100.jpg"  class="image-fit" z-index=0>
      <div class="imgBlur">
        <div style="padding: 20rem;">
          <p style="color:white; font-size: 1.5rem; opacity=1">도아의 돌잔치에<br>초대합니다</p>
        </div>
        
      </div>
    </div>
    <!-- 인사? -->
    <div>
      <p>세상에서 가장 큰 선물로 우리에게 와준<br>도아쨩</p>
    </div>
    <!-- 달력? -->
    <div>
      <b-calendar
        id="calendar"
        :readonly="readonly"
      >

      </b-calendar>
    </div>
    <!-- 지도 -->
    <div class="dirSection">
      <p>위치</p>
      <div style="width:100%">
        <div id="map" style="width:50%;height:450px;margin: auto"></div>  
        <b-button squared
          style="width:50%; margin: auto; display:grid"
          href="https://map.naver.com/v5/directions/-/14138002.419104088,4516506.529278252,%EC%84%9C%EC%9A%B8%ED%81%B4%EB%9F%BD,20202001,PLACE_POI/-/transit?c=15,0,0,0,dh">지도 열기</b-button>

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

    <!-- 사진 -->
      
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
export default {
  name: 'IndexPage',
  mounted(){    
    // 네이버 지도 추가
    const script = document.createElement('script');
    script.src = 'https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=i1bmdsyj4j';
    
    script.addEventListener("load", () =>{
      var map = new naver.maps.Map('map',{
        center: new naver.maps.LatLng(37.5532645, 127.0038206), //지도의 초기 중심 좌표
        zoom: 15, 
         minZoom: 7, //지도의 최소 줌 레벨
        zoomControl: true, //줌 컨트롤의 표시 여부
        zoomControlOptions: { //줌 컨트롤의 옵션
            position: naver.maps.Position.TOP_RIGHT
        }
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
    } 
  // methods:{

  // }
  },
}
</script>
<style>
#container {
  width: 100%;
  height: 100vh;
  position: relative;
  outline: none;
  flex: 1 1 0%;
  background-color: rgb(232, 232, 232);
  overflow-y: auto;
}
#container::-webkit-scrollbar {
  display: none;
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



/* 연락처 */
.contactSection{
  display: flex;
  flex-direction: row;
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

