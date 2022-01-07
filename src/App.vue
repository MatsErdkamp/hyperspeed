<template>
  <div>
    <navbar />
    <hyperspeed :isProcessing="isProcessing" :ETH="ETH" :LINK="LINK" />
    <swap-interface
      @buttonPressed="startSwap"
      :isProcessing="isProcessing"
      :LINKETH="LINKETH"
    ></swap-interface>
  </div>
</template>

<script>
import Hyperspeed from "./components/Hyperspeed.vue";
import Navbar from "./components/Navbar.vue";
import SwapInterface from "./components/SwapInterface.vue";

export default {
  components: { Hyperspeed, SwapInterface, Navbar },
  name: "App",
  data() {
    return {
      isProcessing: false,
      LINKETH: 0,
      LINK: 0,
      ETH: 0,
    };
  },
  methods: {
    startSwap: function (val) {
      console.log("swapping!!");
      this.isProcessing = !this.isProcessing;
      this.ETH = val['ETH'];
      this.LINK = val['LINK'];
    },
  },
  mounted() {
    fetch(
      "https://api.coingecko.com/api/v3/simple/price?ids=chainlink&vs_currencies=eth"
    )
      .then((response) => response.json())
      .then((data) => {
        this.LINKETH = data["chainlink"]["eth"];
      });
  },
};
</script>

<style>
html {
  background: black;
}

body {
  margin: 0;
}

#app {
  font-family: "Roboto Mono", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
