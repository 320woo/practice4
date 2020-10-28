<template>
  <v-app id="inspire">
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
            <v-list-tile-title>지도</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile>
          <v-list-tile-action>
            <v-icon>event</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>일정</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar color="indigo" dark fixed app>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>Vuetify</v-toolbar-title>
    </v-toolbar>
    <v-content class="display-1 font-weight-black">
      내 위치
      <v-container class="display-1 font-weight-black pl-4 align-center">
        <v-layout row
                  justify-center
                  align-center
        >
          <v-flex md1>
          </v-flex>
          <v-flex md10>
            <vl-map
              :load-tiles-while-animating="true"
              :load-tiles-while-interacting="true"
              data-projection="EPSG:4326"
              style="height: 550px"
              ref="map"
            >
              <v-btn
                color="pink"
                dark
                small
                absolute
                fab
                style="margin-top: 150px; margin-left: 27px;"
                @click="setPositionCurrent()"
              >
                <v-icon>mdi-map-marker-radius-outline</v-icon>
              </v-btn>
              <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation"></vl-view>

              <vl-geoloc @update:position="geolocPosition = $event">
                <template slot-scope="geology">
                  <vl-feature v-if="geology.position" id="position-feature">
                    <vl-geom-point :coordinates="geology.position"></vl-geom-point>
                    <vl-style-box>
                      <vl-style-icon
                        :src="require('@/assets/marker.png')"
                        :scale="0.1"
                        :anchor="[0.5, 1]"
                      ></vl-style-icon>
                    </vl-style-box>
                  </vl-feature>
                </template>
              </vl-geoloc>
              <vl-layer-tile id="osm">
                <vl-source-osm></vl-source-osm>
              </vl-layer-tile>
            </vl-map>
          </v-flex>
          <v-flex md1>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    zoom: 2,
    center: [0, 0],
    absolute: true,
    overlay: false,
    drawer: null,
    rotation: 0,
    geolocPosition: undefined
  }),
  props: {
    source: String
  },
  methods: {
    setPositionCurrent () {
      this.center = this.geolocPosition
      this.zoom = 17
    }
  }

}
</script>
