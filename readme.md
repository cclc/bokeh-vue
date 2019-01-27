## Show bokeh chart in vue

### Installation
```
npm i bokeh-vue --save
```

### Usage
```javascript
<template>
  <div>
    <bokeh-vue :plot="plot"></bokeh-vue>
  </div>
</template>

<script>
import BokehVue from 'bokeh-vue'
export default {
  components: {
    'bokeh-vue': BokehVue
  },
  data () {
    plot: {}
  },
  mounted () {
    this.plot = {
      'bokehVersion': '1.0.2',
      'div': '["<div class="bk-root" id="74694b03-e536-4de1-b629-e5c7cc63a5fa"></div>"]',
      'script': '["(function() {↵  var fn = function() {↵    Bokeh.sa…t.addEventListener("DOMContentLoaded", fn);↵})();"]'
    }
  }
}
</script>
```
You can set the value of 'bokehVersion' or it defaults to 1.0.2

The type of 'div' and 'script' is array


### Backend python code
```python
script, div = components(p, wrap_script=False)
return {'div':div, 'script':script}
```