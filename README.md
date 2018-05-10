# vue-waterfall-easy
1. This is a vue component that contains waterfall flow layout and infinite scroll loading
2. Compared to other implementations,there is no need to specify the width and height of the image in the returned data
3. It is because of the second item that the image is preloaded and then layout
4. Responsive layout,adapt mobile
5. Simple to use

## [中文文档](https://github.com/lfyfly/vue-waterfall-easy/blob/master/README-CN.md)
## [Demo](https://lfyfly.github.io/vue-waterfall-easy/demo/)
## [code of demo](https://github.com/lfyfly/vue-waterfall-easy/blob/master/src/App.vue)
## [github](https://github.com/lfyfly/vue-waterfall-easy)

## 1. Usage
### 1.1 Installation

```
npm install vue-waterfall-easy --save
```

### 1.2 es6 import
```js
import vueWaterfallEasy from 'vue-waterfall-easy'
```
```js

export default {
  name: 'app',
  components: {
    vueWaterfallEasy
  }
}
```
### 1.3 scripts import
[download vueWaterfallEasy.js](https://github.com/lfyfly/vue-waterfall-easy/blob/master/src/vue-waterfall-easy/script/vueWaterfallEasy.js)
```html
<script src="path/to/vue/vue.js"></script>
<script src="path/to/vueWaterfallEasy.js"></script>
```
```js
new Vue({
  el: '#app',
  components: {
    vueWaterfallEasy
  }
})
```
### 1.4 Supports require.js and sea.js module import

## 2. Basic example
html
```html
<vue-waterfall-easy :imgsArr="imgsArr" @scrollReachBottom="getData"></vue-waterfall-easy>
```


**getData method new request returned data should merged with the original data**

js

```js
import vueWaterfallEasy from './vue-waterfall-easy/vue-waterfall-easy.vue'
import axios from 'axios'
export default {
  name: 'app',
  data() {
    return {
      imgsArr: [],
      group: 0,// request param
    }
  },
  components: {
    vueWaterfallEasy
  },
  methods: {
    getData(group) {
      // In the real environment,the backend will return a new image array based on the parameter group.
      // Here I simulate it with a stunned json file.
      axios.get('./static/mock/data.json?group=' + this.group)
        .then(res => {
          this.imgsArr = this.imgsArr.concat(res.data)
          group++
        })
    },
  },
  created() {
    this.getData()
  }
}
```
[more detail see App.vue file](https://github.com/lfyfly/vue-waterfall-easy/blob/master/src/App.vue)

## 3. Props
props | type | default | description
---|---|---|---
width | Number |  - | Container width,default is 100% relative parent element width,**Due to the responsiveness,all its parent's width must be 100% relative to the browser window at this time**,See the example after the table<br>**If it is fixed width, you must set the width prop **, not just its parent element set fixed width
height | Number | - | Container height,default is 100% relative parent element height<br>**The parent element must have a height when the height prop is not passed**
gap | Number | 20 | space between pictures(px)
imgsArr | Array | [] | **required**<br>Data used to render the waterfall stream<br>Each array element is an object and must have src and href attributes.<br>The src attribute represents the SRC attribute of the picture<br>The href attribute represents the link to click to jump
imgWidth | Number | 240 | The width of the picture（px）
maxCols | Number | 5 | Waterfall shows the maximum number of columns
linkRange | String | card | Identify click to trigger jump link range<br>value:<br>'card' Whole card range<br> 'img' image range <br> 'custom' Customize the link range through slots
reachBottomDistance | Number | 0 | The distance(px) from the bottom of the container when the scrolling triggers the scrollReachBottom event
loadingDotCount | Number | 3 | The number of default loading animation dots
loadingDotStyle | Object | null | The style object of the small dots in the default loading element
loadingTimeOut | Number | 500 |  Preloading events less than 500ms milliseconds do not display loading animations,increasing the user experience




## 4. Event
event name | description
---|---
scrollReachBottom | When the scroll bar scrolls to the bottom,it is used to trigger a request for new image data
preloaded | Trigger every time image preloading is completed

## 5. slot
### 5.1 default slot
Custom picture description element
#### parameter
parameterpar | description
---|---
props.index | The index of the image in the data array,starting from 0
props.value | The value of imgsArr item

```html
<vue-waterfall-easy :imgsArr="imgsArr" @scrollReachBottom="getData">
  <div class="img-info" slot-scope="props">
    <p class="some-info">picture index: {{props.index}}</p>
    <p class="some-info">{{props.value.info}}</p>
  </div>
</vue-waterfall-easy>
```
### 5.2 slot="loading"
Custom loading element
```html
<div slot="loading" slot-scope="{isFirstLoad}">
  <div slot="loading" v-if="isFirstLoad">first-loading...</div>
  <div v-else="v-else">loading...</div>
</div>
```
### 5.3 slot="waterfall-head"
Waterfall container head slot
```html
<vue-waterfall-easy :imgsArr="imgsArr" @scrollReachBottom="getData">
  <div slot="waterfall-head">waterfall-head</div>
</vue-waterfall-easy>
```



## 6. Adapted mobile
Don't forget to add following  code in index.html \<head\>
```html
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
```


