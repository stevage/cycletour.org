<template>
  <div id="app">
    <div id='map'></div>
  </div>

</template>

<script>
// const mapboxgl = require('mapbox-gl');
import mapboxgl from 'mapbox-gl';
import MapboxDirections from '@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions';
import { MapboxStyleSwitcherControl } from "mapbox-gl-style-switcher/dist/index";
import "mapbox-gl-style-switcher/styles.css";
const MD = MapboxDirections;
// import { MapboxStyleDefinition, MapboxStyleSwitcherControl } from "mapbox-gl-style-switcher";
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
  window.map = map;
  map.addControl(new MapboxDirections({
      accessToken: mapboxgl.accessToken
  }), 'top-left');
  // window.dl = require('@mapbox-gl-directions/dist/
  let directionsLayers = [], directionsSource = {};
  map.on('styledata', (e) => {
    console.log(e);
    const curStyle = map.getStyle();
    if (directionsLayers.length === 0 && curStyle.sources['directions']) {
      directionsLayers = curStyle.layers.filter(l => l.source === 'directions');
      directionsSource = curStyle.sources['directions'];
      window.directionsLayers = directionsLayers;
    } else if (directionsLayers.length > 0) {
      const newStyle = e.style.stylesheet;//map.getStyle();
      if (!map.getSource('directions')) {
          if (!map.getSource('directions')) {
            // TODO: somehow preserve the old contents of this layer, so we don't lose directions when switching layer.
            map.addSource('directions', directionsSource);
          }
          directionsLayers.forEach(l => {
            if (!map.getLayer(l)) {
              map.addLayer(l);
            }
          });
      }
    }
  });
  
  const oldCycletour = {
    "version": 8,
    "sources": {
        "mapbox://stevage.2xuscm8g": {
            "url": "mapbox://stevage.2xuscm8g",
            "type": "raster",
            "tileSize": 128
        }
    },
    "sprite": "mapbox://sprites/stevage/cjqkcw8mw2ym32smvt9wsh4c4",
    "glyphs": "mapbox://fonts/stevage/{fontstack}/{range}.pbf",
    "layers": [
        {
            "id": "background",
            "type": "background",
            "paint": {
                "background-color": "rgba(0,0,0,0)"
            }
        },
        {
            "type": "raster",
            "paint": {
                "raster-resampling": "nearest",
            },
            "layout": {},
            "id": "stevage-2xuscm8g",
            "source": "mapbox://stevage.2xuscm8g"
        }
    ]
}


  const styles = [
    {
        title: "Outdoors",
        uri:'mapbox://styles/stevage/cjopcl1j9555x2rt298enfwhf'
    },
    {
        title: "Light",
        uri:"mapbox://styles/mapbox/light-v9"
    }, {
      title: 'Old cycletour',
      uri: oldCycletour
    }, {
      title: 'New cycletour',
      uri: 'mapbox://styles/stevage/cjqk6ve931h5o2spctaxzuu4h'
    }
];

  map.addControl(new MapboxStyleSwitcherControl(styles));
  window.map = map;
}

/*
Can get route geojson: map.getSource('directions')._data
*/

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
