<template>
  <div>
    <div v-for="(item,index) in plot.div" v-html="item" :key="index"></div>
  </div>
</template>

<script>
export default {
  props: {
    plot: {
      bokehVersion: "",
      div: [],
      script: []
    }
  },
  mounted() {},
  watch: {
    plot: function(val, oldVal) {
      this.addCss(val.bokehVersion);
      const _this = this;
      this.loadScript(
        "https://cdn.pydata.org/bokeh/release/bokeh-" +
          val.bokehVersion +
          ".min.js",
        function() {
          // draw image
          const val = _this.plot;
          if (val.script && val.script.length > 0) {
            for (var i = 0; i < val.script.length; i++) {
              var bokehRunScript = document.createElement("SCRIPT");
              bokehRunScript.setAttribute("type", "text/javascript");
              var t = document.createTextNode(val.script[i]);
              bokehRunScript.appendChild(t);
              document.body.appendChild(bokehRunScript);
            }
          }
        }
      );
    }
  },
  methods: {
    addCss(bokehVersion) {
      var link = document.createElement("link");
      link.setAttribute("rel", "stylesheet");
      link.setAttribute(
        "href",
        "https://cdn.pydata.org/bokeh/release/bokeh-" +
          bokehVersion +
          ".min.css"
      );
      link.setAttribute("type", "text/css");
      document.head.appendChild(link);
    },
    loadScript(src, callback) {
      var script = document.createElement("script"),
        head = document.getElementsByTagName("head")[0];
      script.type = "text/javascript";
      script.charset = "UTF-8";
      script.src = src;
      if (script.addEventListener) {
        script.addEventListener(
          "load",
          function() {
            callback();
          },
          false
        );
      } else if (script.attachEvent) {
        script.attachEvent("onreadystatechange", function() {
          var target = window.event.srcElement;
          if (target.readyState == "loaded") {
            callback();
          }
        });
      }
      head.appendChild(script);
    }
  }
};
</script>
