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
  mounted() {
    // this.plot.bokehVersion = "1.0.2"; // use the newest version
    // var link = document.createElement("link");
    // link.setAttribute("rel", "stylesheet");
    // link.setAttribute(
    //   "href",
    //   "https://cdn.pydata.org/bokeh/release/bokeh-" +
    //     this.plot.bokehVersion +
    //     ".min.css"
    // );
    // link.setAttribute("type", "text/css");
    // document.head.appendChild(link);
    // this.addCss(this.plot.bokehVersion);
    // var script = document.createElement("script");
    // script.setAttribute(
    //   "src",
    //   "https://cdn.pydata.org/bokeh/release/bokeh-" +
    //     this.plot.bokehVersion +
    //     ".min.js"
    // );
    // script.async = "async";
    // document.head.appendChild(script);
    // this.addScript(this.plot.bokehVersion);
  },
  // monitor plot's change
  watch: {
    plot: function(val, oldVal) {
      // user change bokeh version, remove old version files
      // if (val.bokehVersion && val.bokehVersion !== oldVal.bokehVersion) {
      //   console.log('val=  ', val, 'oldVal=  ', oldVal)
      //   this.removeCss(oldVal.bokehVersion);
      //   this.removeScript(oldVal.bokehVersion);
      //   this.addCss(val.bokehVersion);
      //   this.addScript(val.bokehVersion);
      //   this.plot.bokehVersion = val.bokehVersion
      //   setTimeout(() => {
      //     this.draw(val)
      //   }, 1);
      // }

      this.addCss(val.bokehVersion);
      const _this = this;
      this.loadScript(
        "https://cdn.pydata.org/bokeh/release/bokeh-" +
          val.bokehVersion +
          ".min.js",
        function() {
          // draw image
          const val = _this.plot;
          // console.log(val,'   ',_this.plot)
          // if (val.div && val.div.length > 0) {
          //   _this.plot.div = val.div;
          // }
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
    draw(val) {
      // draw image
      if (val.div && val.div.length > 0) {
        this.plot.div = val.div;
      }
      if (val.script && val.script.length > 0) {
        for (var i = 0; i < val.script.length; i++) {
          var bokehRunScript = document.createElement("SCRIPT");
          bokehRunScript.setAttribute("type", "text/javascript");
          var t = document.createTextNode(val.script[i]);
          bokehRunScript.appendChild(t);
          document.body.appendChild(bokehRunScript);
        }
      }
    },
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
    addScript(bokehVersion) {
      var script = document.createElement("script");
      script.setAttribute(
        "src",
        "https://cdn.pydata.org/bokeh/release/bokeh-" + bokehVersion + ".min.js"
      );
      script.async = "async";
      document.head.appendChild(script);
    },
    removeCss(bokehVersion) {
      var href =
        "https://cdn.pydata.org/bokeh/release/bokeh-" +
        bokehVersion +
        ".min.css";
      var links = document.getElementsByTagName("link");
      for (var i = 0; i < links.length; i++) {
        var _href = links[i].href;
        if (links[i] && links[i].href && links[i].href.indexOf(href) != -1) {
          links[i].parentNode.removeChild(links[i]);
        }
      }
    },
    removeScript(bokehVersion) {
      var src =
        "https://cdn.pydata.org/bokeh/release/bokeh-" +
        bokehVersion +
        ".min.js";
      var scripts = document.getElementsByTagName("script");
      for (var i = 0; i < scripts.length; i++) {
        if (scripts[i] && scripts[i].src && scripts[i].src.indexOf(src) != -1) {
          scripts[i].parentNode.removeChild(scripts[i]);
        }
      }
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
