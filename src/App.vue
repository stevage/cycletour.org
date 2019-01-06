<template>
  <div id="app">
    <div id='map'></div>
  </div>

</template>

<script>
// const mapboxgl = require('mapbox-gl');
import mapboxgl from 'mapbox-gl';
import MapboxDirections from '@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions';
export default {
  name: 'app',
  components: {
  }, mounted() {
    initMap();
  }
}

function initMap() {
  mapboxgl.accessToken = 'pk.eyJ1Ijoic3RldmFnZSIsImEiOiJGcW03aExzIn0.QUkUmTGIO3gGt83HiRIjQw';
  var map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/stevage/cjopcl1j9555x2rt298enfwhf',
      center: [148,-36],
      zoom: 11
  });
  map.addControl(new MapboxDirections({
      accessToken: mapboxgl.accessToken
  }), 'top-left');
  window.map = map;
}

require('../node_modules/mapbox-gl/dist/mapbox-gl.css');
require('../node_modules/@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions.css');

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}

body { margin:0; padding:0; }
#map { position:absolute; top:0; bottom:0; width:100%; }

</style>
