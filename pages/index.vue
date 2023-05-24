<template>
  <div id="container">
    <div>
      <div class="mainSection">
        <img src="../assets/doahJoah_100.jpg"  class="image-fit" z-index=0>
        <div class="imgBlur">
          <div style="padding: 20rem;">
            <p style="color:white; font-size: 1.5rem; opacity=1">도아의 돌잔치에</br>초대합니다</p>
          </div>
          
        </div>
      </div>
      <div>
        <p>세상에서 가장 큰 선물로 우리에게 와준 </br>도아쨩</p>
      </div>
    </div>
    <div class="dirSection">
      <div id="map" style="width:50%;height:450px;margin: 5rem auto"></div>  
    </div>
    

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
        center: new naver.maps.LatLng(37.3595704, 127.105399), //지도의 초기 중심 좌표
        zoom: 15, 
         minZoom: 7, //지도의 최소 줌 레벨
        zoomControl: true, //줌 컨트롤의 표시 여부
        zoomControlOptions: { //줌 컨트롤의 옵션
            position: naver.maps.Position.TOP_RIGHT
        }
      })
      var marker = new naver.maps.Marker({
        position: new naver.maps.LatLng(37.3595704, 127.105399),
        map: map
      })
    });
    
    document.head.appendChild(script);
    
  },
  data(){
    return{
      lat: 0,
      long: 0,
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
</style>>

</style>

