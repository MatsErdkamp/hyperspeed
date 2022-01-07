<template>
  <div class="hyperspeed-container" id="hyperspeed-container">
    <transition name="fade" mode="out-in">
      <processing
        v-show="hexagonFullScale"
        :isCompleted="isCompleted"
        :ETH="ETH"
        :LINK="LINK"
      />
    </transition>
  </div>
</template>

<script>
import {
  SVG,
  extend as SVGextend,
  Element as SVGElement,
} from "@svgdotjs/svg.js";
import Processing from "./Processing.vue";

export default {
  components: { Processing },
  props: ["isProcessing", "LINK", "ETH"],
  data() {
    return {
      draw: [],
      centerX: 0,
      centerY: 0,
      centerHexagon: [],
      hexagonArray: [],
      animate: false,
      hexagonFullScale: false,
      transactionCompleted: false,
      isCompleted: false,
    };
  },
  methods: {
    initializeHyperSpeed: function () {
      let self = this;
      var loop = 0;

      for (var i = 0; i < 150; i++) {
        self.spawnHexagon(this.draw, loop);
        loop += 1;
      }

      this.centerHexagon = this.draw
        .polygon("300,150 225,280 75,280 0,150 75,20 225,20")
        .fill("none")
        .stroke({ width: 2, color: "#fff" })
        .move(this.centerX - 150, this.centerY - 150)
        .transform({ rotate: 90 })
        .scale(0.3);
    },

    initialAnimation: function () {
      var self = this;
      let loop1 = 0;

      function animateFirstHexagon() {
        self.centerHexagon.scale(1 + 1 / loop1 ** 2.11);

        // Start next frame
        loop1 += 1;
        if (loop1 < 50) {
          requestAnimationFrame(animateFirstHexagon);
        } else {
          animateHexagons();

          setTimeout(() => {
            self.hexagonFullScale = true;
          }, 1500);
        }
      }

      animateFirstHexagon();

      let loop2 = -10;

      function animateHexagons() {
        self.hexagonArray.forEach((element, index) => {
          var lightness = Math.min(10 - index / 10, loop2 - index);

          element.stroke({
            color: `hsl(226, 70%, ${lightness}%)`,
          });
        });

        // Start next frame
        loop2 += 1;
        if (loop2 < 140) {
          requestAnimationFrame(animateHexagons);
        } else {
          self.animateHyperSpeed();
        }
      }
    },
    animateHyperSpeed: function () {
      console.log("animating!!");

      let self = this;
      var loop = -40;

      function animate() {
        self.hexagonArray.forEach((element, index) => {
          var lightness = Math.max(
            10 - index / 10,
            100 - index / 1.2 - Math.abs(index - loop) ** 1.3
          );

          element.stroke({
            width: 1,
            color: `hsl(226, ${80 - lightness}%, ${lightness}%)`,
          });
        });

        // Start next frame
        loop += 1;
        if (loop > 140) {
          loop = -40;

          if (self.transactionCompleted == true) {
            self.animate = false;
            self.animateTransactionCompleted();
          }
        }
        if (self.animate == true) {
          requestAnimationFrame(animate);
        }
      }

      animate();
    },
    animateTransactionCompleted: function () {
      console.log("completed!!");
      let loop = 1;
      let self = this;

      function animate() {
        self.hexagonArray.at(-loop).stroke({
          width: 1,
          color: `hsl(152, ${loop - 30}%, ${loop - 60}%)`,
        });

        // Start next frame
        loop += 1;
        if (loop < 150) {
          requestAnimationFrame(animate);
        } else {
          self.isCompleted = true;
          self.centerHexagon.fill("hsl(152,60%,70%)").stroke({ width: 0 });
        }
      }

      animate();
    },
    spawnHexagon: function (draw, loop) {
      let polygon = draw
        .polygon("300,150 225,280 75,280 0,150 75,20 225,20")
        .fill("none")
        .stroke({ width: 1 })
        .move(this.centerX - 150, this.centerY - 150)
        .transform({ rotate: 90 })
        .scale(1 + loop / 15)
        .rotate(loop * 1.618);

      polygon["spawnTime"] = loop;

      this.hexagonArray.push(polygon);
    },
  },
  mounted() {
    this.centerX = window.innerWidth / 2;
    this.centerY = window.innerHeight / 2;
    this.draw = SVG().addTo("#hyperspeed-container").size("100%", "100%");
    this.initializeHyperSpeed();


    window.addEventListener("keydown", (event) => {
      if (event.key == "e") {
        this.transactionCompleted = true;
      }
    });
  },
  watch: {
    isProcessing: function (val) {
      if (val == true) {
        this.animate = true;

        let self = this;
        setTimeout(() => {
          self.initialAnimation();
        }, 500);
        setTimeout(() => {
          self.transactionCompleted = true;
        }, 10000);
      } else {
        this.animate = false;
      }
    },
  },
};
</script>

<style>
.hyperspeed-container {
  height: 100vh;
}
</style>