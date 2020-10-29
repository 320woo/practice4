<template>
  <v-app id="inspire" class="services">
    <v-navigation-drawer
      v-model="drawer"
      fixed
      app
      width="200px"
    >
      <v-list dense>
        <v-list-tile>
          <v-list-tile-action>
            <v-icon>map</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <router-link class="nav-link" to="/services">지도</router-link>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile>
          <v-list-tile-action>
            <v-icon>event</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <router-link class="nav-link" to="/blog">일정</router-link>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <router-view/>
  </v-app>
</template>
<script>
import 'ol/ol.css';
import Feature from 'ol/Feature';
import Geolocation from 'ol/Geolocation';
import Map from 'ol/Map';
import Point from 'ol/geom/Point';
import View from 'ol/View';
import {Circle as CircleStyle, Fill, Stroke, Style} from 'ol/style';
import {OSM, Vector as VectorSource} from 'ol/source';
import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';

export default {
  async mounted () {
    await this.initiateMap()
  },
  methods: {
    initiateMap () {
      const view = new View({
        center: [0, 0],
        zoom: 2,
      })

      const map = new Map({
        layers: [
          new TileLayer({
            source: new OSM(),
          })],
        target: 'map',
        view: view,
      })

      const geolocation = new Geolocation({
        // enableHighAccuracy must be set to true to have the heading value.
        trackingOptions: {
          enableHighAccuracy: true,
        },
        projection: view.getProjection(),
      })

      function el(id) {
        return document.getElementById(id);
      }
      el('track').addEventListener('change', function () {
        geolocation.setTracking(this.checked);
      });

      geolocation.on('error', function (error) {
        const info = document.getElementById('info')
        info.innerHTML = error.message;
        info.style.display = '';
      });

      const accuracyFeature = new Feature()
      geolocation.on('change:accuracyGeometry', function () {
        accuracyFeature.setGeometry(geolocation.getAccuracyGeometry());
      });

      const positionFeature = new Feature()
      positionFeature.setStyle(
        new Style({
          image: new CircleStyle({
            radius: 6,
            fill: new Fill({
              color: '#3399CC',
            }),
            stroke: new Stroke({
              color: '#fff',
              width: 2,
            }),
          }),
        })
      );
      geolocation.on('change:position', function () {
        const coordinates = geolocation.getPosition()
        positionFeature.setGeometry(coordinates ? new Point(coordinates) : null);
      });
      new VectorLayer({
        map: map,
        source: new VectorSource({
          features: [accuracyFeature, positionFeature],
        }),
      });
    }
  }
}
</script>
<style>
#map {
  position: center;
  margin: 0;
  padding: 0;
  height: 400px;
  width: 100%;
}
</style>
