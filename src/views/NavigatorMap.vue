<template>
    <div class="main-container">
        
        <pre id="info"></pre>
        <div id="map"></div>
        
        navigator
    </div>    
</template>

<script>
import * as mapboxgl from 'mapbox-gl/dist/mapbox-gl.js';
mapboxgl.accessToken = 'pk.eyJ1IjoibW9za2FsZXZpY2giLCJhIjoiY2tkOGpkejVoMDVsczJ6bzFwN3JlcWdleiJ9.BPJATv3yOVNPi_Jx7y7TvQ';

export default {
    data: () => ({
        map: null,
    }),
    mounted() {
        this.map = new mapboxgl.Map({
            container: 'map',
            style: 'mapbox://styles/mapbox/light-v10',
            center: [27.577528953552246, 53.91891122792043],
            zoom: 15.5,
            pitch: 45,
            bearing: -17.6,
            antialias: true
        });
        this.fullScrinControl();
        this.addMouseCoords();
    },
    methods: {
        fullScrinControl() {
            this.map.addControl(new mapboxgl.FullscreenControl());
        },
        addMouseCoords() {
            this.map.on('mousemove', function(e) {
                document.getElementById('info').innerHTML =
                JSON.stringify(e.point) +
                '<br />' +
                JSON.stringify(e.lngLat.wrap());
            });
            this.map.on('click', function(e) {
                console.log(e.lngLat.wrap())
                let lng = e.lngLat.wrap().lng;
                let lat = e.lngLat.wrap().lat;

                let el = document.createElement('div');
                    el.className = 'marker';

                new mapboxgl.Marker(el)
                    .setLngLat([lng, lat])
                    .addTo(this)
            });
        },
    },
}
</script>

<style lang="scss" scoped>
.main-container {
    z-index: 1;
    width: 100vw;
}

#map {
    width: 80%;
    height: 80%;
}

#info {
    display: block;
    position: relative;
    margin: 0px auto;
    width: 50%;
    padding: 10px;
    border: none;
    border-radius: 3px;
    font-size: 12px;
    text-align: center;
    color: #222;
    background: #fff;
}

.marker {
  background-image: url('https://img.icons8.com/ios/50/000000/detain.png');
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 2;
}
</style>