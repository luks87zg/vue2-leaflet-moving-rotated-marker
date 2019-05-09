# vue2-leaflet-moving-rotated-marker

This is a [moving-rotated-marker plugin](https://github.com/luks87zg/Leaflet.MovingRotatedMarker) extension for [vue2-leaflet package](https://github.com/KoRiGaN/Vue2Leaflet)

## Install
```bash
npm install --save vue2-leaflet-moving-rotated-marker
```

## Demo

```bash
git clone git@github.com:luks87zg/vue2-leaflet-moving-rotated-marker.git
npm install
npm run example
```

Then you should be able to navigate with your browser and see the demo in http://localhost:4000/

You can see the demo code in the file [example.vue](example.vue)

## Usage

### on &lt;template&gt; add

something like this
```html
<l-map :zoom=10 :center="initialLocation">
  <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png" />
  <l-moving-rotated-marker
      v-for="driver in drivers"
      :key="driver.uuid"
      v-if="driver.location"
      :lat-lng="getLocation(driver)"
      :icon="getIcon(driver.uuid)"
      @click="setCurrentDriver(driver)"
      ref="driverMarker"
      :duration="2000"
      :rotationAngle="45"
  />
</l-map>
```
### on &lt;script&gt; add

#### option 1

In the same template file, at `<script>` part, this will make the component available only to the template in this file

```js
import LMovingRotatedMarker from 'vue2-leaflet-moving-rotated-marker'
...
export default {
  ...
  components: {
    LMovingRotatedMarker
    ...
  },
  ...
}
```
#### option 2

At main Vue configuration, this will make the component available to all templates in your app
```js
import Vue from 'vue'
import LMovingRotatedMarker from 'vue2-leaflet-moving-rotated-marker'
...
Vue.component('l-moving-rotated-marker', LMovingRotatedMarker)
```

## Access movingmarker layer directly

If you need to access other movingmarker methods, like [slideTo()](https://gitlab.com/movingmarker/Leaflet.Marker.SlideTo), you can do it with a ref on the movingmarker vue element and using the `mapObject` property

```html
...
<l-moving-rotated-marker ref="movingRotatedMarkerRef">
  ...
</l-moving-rotated-marker>
...
```
```js
...
this.$refs.movingRotatedMarkerRef.mapObject.slideTo()
...
```


## Develop and build

    npm install
    npm run build


## License

MIT
