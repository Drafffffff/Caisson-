<template>
  <div id="app">
    <div id="canvas" class="bg-gray-100"></div>

    <div class="control-outer flex-1">
      <div class="outer" v-if="step === 1">
        <p class="text-center py-4 text-xl">选择垂幔</p>
        <div class="select grid grid-cols-3 gap-4 p-2">
          <div class @click="outerChange(1)">
            <img src="./assets/outer/1.svg" alt />
          </div>
          <div class @click="outerChange(2)">
            <img src="./assets/outer/2.svg" alt />
          </div>
          <div class @click="outerChange(3)">
            <img src="./assets/outer/1.svg" alt />
          </div>
          <div class @click="outerChange(4)">
            <img src="./assets/outer/1.svg" alt />
          </div>
          <div class @click="outerChange(5)">
            <img src="./assets/outer/1.svg" alt />
          </div>
          <div class @click="outerChange(6)">
            <img src="./assets/outer/1.svg" alt />
          </div>
        </div>
        <div class="confirm py-10 px-4 flex justify-between">
          <button
            class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            @click="outerChange(0)"
          >
            清除
          </button>
          <button
            class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            @click="step = 2"
          >
            下一步
          </button>
        </div>
      </div>
      <div class="midder" v-if="step === 2">
        <p class="text-center py-4 text-xl">选择边饰</p>
        <div class="select grid grid-cols-3 gap-4 p-2">
          <div class @click="midderChange(1)">
            <img src="./assets/midder/2.svg" alt />
          </div>
          <div class @click="midderChange(2)">
            <img src="./assets/midder/1.svg" alt />
          </div>
          <div class @click="midderChange(3)">
            <img src="./assets/midder/3.svg" alt />
          </div>
          <div class @click="midderChange(4)">
            <img src="./assets/midder/1.svg" alt />
          </div>
          <div class @click="midderChange(5)">
            <img src="./assets/midder/1.svg" alt />
          </div>
          <div class @click="midderChange(6)">
            <img src="./assets/midder/1.svg" alt />
          </div>
        </div>
        <div class="confirm py-10 px-4 flex justify-between">
          <button
            class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            @click="
              midderChange(0);
              midderChange(0);
              midderChange(0);
            "
          >
            清除
          </button>
          <div class="step">
            <button
              class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
              @click="step = 1"
            >
              上一步
            </button>
            <button
              class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
              @click="step = 3"
            >
              下一步
            </button>
          </div>
        </div>
      </div>
      <div class="inner" v-if="step === 3">
        <p class="text-center py-4 text-xl">选择井心</p>
        <div class="select grid grid-cols-3 gap-4 p-2">
          <div class @click="innerChange(1)">
            <img src="./assets/inner/1.svg" alt />
          </div>
          <div class @click="innerChange(2)">
            <img src="./assets/inner/2.svg" alt />
          </div>
          <div class @click="innerChange(3)">
            <img src="./assets/inner/1.svg" alt />
          </div>
          <div class @click="innerChange(4)">
            <img src="./assets/inner/1.svg" alt />
          </div>
          <div class @click="innerChange(5)">
            <img src="./assets/inner/1.svg" alt />
          </div>
          <div class @click="innerChange(6)">
            <img src="./assets/inner/1.svg" alt />
          </div>
        </div>
        <div class="confirm py-10 px-4 flex justify-between">
          <button
            class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            @click="innerChange(0)"
          >
            清除
          </button>
          <div class="step">
            <button
              class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
              @click="step = 2"
            >
              上一步
            </button>
            <button
              class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            >
              完成
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import drawShape from "./assets/shapes.js";
import { SVG } from "@svgdotjs/svg.js";

export default {
  name: "App",
  components: {},
  data() {
    return {
      shapes: {},
      step: 1,
      layer: {
        outer: 0,
        midder: [0, 0, 0],
        inner: 0,
      },
    };
  },
  mounted() {
    this.shapes = new drawShape(
      SVG()
        .addTo("#canvas")
        .size(this.width, this.width)
    );
    this.shapes.width = this.width;
    // this.shapes.inner6();
  },
  methods: {
    outerChange(n) {
      this.layer.outer = n;
      this.updateSVG();
    },
    midderChange(n) {
      this.layer.midder.push(n);
      this.layer.midder = this.layer.midder.splice(1, 3);
      this.shapes.midder0();
      this.updateSVG();
    },
    innerChange(n) {
      this.cleanSVG();
      this.layer.inner = n;
      console.log(this.layer.inner)
      this.updateSVG();
    },
    cleanSVG() {
      this.layer.outer = 0;
      this.layer.inner = 0;
      this.layer.midder = [0, 0, 0];
      this.updateSVG();
    },
    updateSVG() {
      let drawOuter = "outer" + this.layer.outer.toString();
      let drawMidder = [];
      for (let i = 0; i < 3; i++) {
        drawMidder.push("midder" + this.layer.midder[i].toString());
      }
      let drawInner = "inner" + this.layer.inner.toString();
      for (let i = 0; i < 3; i++) {
        this.shapes[drawMidder[i]]();
      }
      this.shapes[drawOuter]();
      this.shapes[drawInner]();
    },
  },
  computed: {
    width: function() {
      return document.getElementById("canvas").offsetWidth;
    },
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  width: 100vw;
}
#control {
  width: 100%;
  height: 100%;
}
#canvas {
  width: 100vmin;
  height: 100vmin;
}
</style>
