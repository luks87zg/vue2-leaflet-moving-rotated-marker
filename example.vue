<template>
  <l-map :zoom="18" :center="center">
    <l-tile-layer
        :url="mapData.url"
        :attribution="mapData.attribution"
    />
    <l-moving-rotated-marker ref="driveMarker"
        :lat-lng="driveLatLng"
        :rotationAngle="driveRotationAngle"
        :duration="3000"
        :icon="icon"
      />
  </l-map>
</template>

<script>
    import * as L from 'leaflet'
    import { LMap, LTileLayer, LIconDefault, LPopup } from 'vue2-leaflet'
    import LMovingRotatedMarker from './Vue2LeafletMovingRotatedMarker'

    const iconUrl= 'data:image/png;base64,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'
    const drivePath= [{"latLng":[45.76740321596149,15.995482206344606],"bearing":90},{"latLng":[45.7674,15.99555],"bearing":93.89030809612814},{"latLng":[45.76684,15.99553],"bearing":-178.5728529280631},{"latLng":[45.76657,15.99532],"bearing":-151.51728462687032},{"latLng":[45.76542,15.99382],"bearing":-137.7003835365391},{"latLng":[45.76485,15.99312],"bearing":-139.41297756803178},{"latLng":[45.76484,15.99312],"bearing":-139.41297756803178},{"latLng":[45.76474,15.99299],"bearing":-137.79547100440942},{"latLng":[45.76454,15.99284],"bearing":-152.3811318692293},{"latLng":[45.76438,15.99277],"bearing":-163.02756418878684},{"latLng":[45.7642,15.99274],"bearing":-173.3680647468638},{"latLng":[45.76362,15.99281],"bearing":175.1873016972113},{"latLng":[45.76326,15.99292],"bearing":167.96675294490683},{"latLng":[45.76291,15.99307],"bearing":163.3541787864533},{"latLng":[45.76276,15.99318],"bearing":152.90588788564162},{"latLng":[45.76258,15.99342],"bearing":137.07168014621476},{"latLng":[45.76231,15.99391],"bearing":128.3029399139998},{"latLng":[45.76222,15.99409],"bearing":125.62929361798263},{"latLng":[45.76211,15.99437],"bearing":119.38482014322068},{"latLng":[45.76207,15.99455],"bearing":107.66841014935528},{"latLng":[45.76203,15.995],"bearing":97.26098336190552},{"latLng":[45.7621,15.9979],"bearing":88.01735047572276},{"latLng":[45.76212,15.99831],"bearing":86.00011356763355},{"latLng":[45.76216,15.99859],"bearing":78.42728881058747},{"latLng":[45.76216,15.99858],"bearing":78.42728881058747},{"latLng":[45.7622,15.99881],"bearing":76.00205855459166},{"latLng":[45.76233,15.99914],"bearing":60.547478477729044},{"latLng":[45.76391,16.00183],"bearing":49.90362092658836},{"latLng":[45.7641,16.00212],"bearing":46.796928715894694},{"latLng":[45.76422,16.00225],"bearing":37.080040353527124},{"latLng":[45.76422,16.00224],"bearing":33},{"latLng":[45.76431,16.00232],"bearing":31.80300792581471},{"latLng":[45.76456,16.00243],"bearing":17.063744344583938},{"latLng":[45.76466,16.00244],"bearing":3.9905295862726575},{"latLng":[45.76475,16.00245],"bearing":4.4322357017290415},{"latLng":[45.76498,16.00241],"bearing":-6.9174475500713015},{"latLng":[45.76519,16.0023],"bearing":-20.072794369281723},{"latLng":[45.7654,16.00209],"bearing":-34.89952228961886},{"latLng":[45.76563,16.00175],"bearing":-45.88071469246705},{"latLng":[45.76635,16.00063],"bearing":-47.337810807420226},{"latLng":[45.76675,16.00002],"bearing":-46.770850436311775},{"latLng":[45.7671,15.99979],"bearing":-24.62706264093606},{"latLng":[45.76728,15.99972],"bearing":-15.177849844461832},{"latLng":[45.7675,15.9997],"bearing":-3.6285874794492656},{"latLng":[45.76823,15.99967],"bearing":-1.642045684338143},{"latLng":[45.76799,15.99114],"bearing":-92.30669640870639},{"latLng":[45.76799,15.99113],"bearing":-89.99999642100522},{"latLng":[45.76799,15.99089],"bearing":-89.99991401704574}]

    export default {
        components: {
            LMap,
            LTileLayer,
            LIconDefault,
            LPopup,
            LMovingRotatedMarker
        },
        data () {
            return {
                icon: L.icon({
                    iconUrl: iconUrl,
                    iconSize: [40,40],
                    iconAnchor: [20,20],
                    popupAnchor: [0,-25]
                }),
                center: L.latLng(45.7674032, 15.995482),
                mapData: {
                    attribution: `&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>`,
                    url: 'https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png'
                },
                driveLatLng: L.latLng(drivePath[0].latLng),
                driveRotationAngle: drivePath[0].bearing,
                drivePath: drivePath.slice(),
                driveMarker:null,
            }
        },
        methods: {
            simulate() {
                if(!this.drivePath.length) {
                    this.$refs.driveMarker.mapObject.setLatLng(L.latLng(drivePath[0].latLng))
                    this.drivePath= drivePath.slice()
                    return
                }
                let point= this.drivePath.shift()
                this.driveLatLng= L.latLng(point.latLng)
                this.driveRotationAngle= point.bearing
            }
        },
        mounted() {
            setInterval(()=> {
                this.simulate()
            },3000)
        }
    }
</script>

<style>
  @import "~leaflet/dist/leaflet.css";
  html, body {
    height: 100%;
    margin: 0;
  }
</style>
