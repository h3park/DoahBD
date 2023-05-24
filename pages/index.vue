<template>
  <div id="container">
    <div>
      <div class="mainSection">
        <img src="../assets/doahJoah_100.jpg"  class="image-fit" z-index=0>
        <div class="imgBlur">
          <div style="padding: 50px;">
            <p style="color:white; font-size: 1.5rem; opacity=1">도아의 돌잔치에</br>초대합니다</p>
          </div>
          
        </div>
      </div>
      <div>
        <p>세상에서 가장 큰 선물로</p>
      </div>
    </div>
    <div class="dirSection">
      <div id="map" ref="map" style="width:100%;height:400px;"></div>  
    </div>
    

  </div>
</template>

<script>
export default {
  beforeMount() {
    let naver = document.createElement('script');
    naver.setAttribute('src',"https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=i1bmdsyj4j");
    document.head.appendChild(naver);

    this.$refs.map = new naver.maps.Map('map',{       
      center: new naver.maps.LatLng(37.3595704, 127.105399),
      zoom: 10
    });
  },
  mounted(){
},
  data(){
    return{
    } 
  methods:{
  }
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

