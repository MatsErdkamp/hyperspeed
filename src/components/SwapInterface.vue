<template>
  <transition name="fade" mode="out-in">
    <div class="swap-container" v-show="isProcessing == false">
      <!-- <small class="swap-element-name">/FROM</small> -->
      <div class="swap-element">
        <div class="swap-element-top">
          <div class="swap-token-selector">
            <small>TOKEN</small>
            <div class="token-selector-input">
              <img class="token-image" src="@/assets/ETHEREUM.jpg" />
              <div class="token-name">ETH</div>
            </div>
          </div>
          <div class="swap-network-selector">
            <small>NETWORK</small>
            <div class="network-selector-input">
              <div class="network-name">ETHEREUM</div>
            </div>
          </div>
        </div>

        <div class="swap-element-bottom">
          <div class="swap-amount-selector">
            <small>AMOUNT</small>
            <input
              id="ETH"
              type="number"
              class="amount-selector-input"
              v-model="ETH"
              placeholder="0"
            />
          </div>
        </div>
      </div>
      <div class="swap-button">⥯</div>
      <div class="swap-element">
        <div class="swap-element-top">
          <div class="swap-token-selector">
            <small>TOKEN</small>
            <div class="token-selector-input">
              <img class="token-image" src="@/assets/LINKTOKEN.png" />
              <div class="token-name">LINK</div>
            </div>
          </div>
          <div class="swap-network-selector">
            <small>NETWORK</small>
            <div class="network-selector-input">
              <div class="network-name">AVALANCHE</div>
            </div>
          </div>
        </div>

        <div class="swap-element-bottom">
          <div class="swap-amount-selector">
            <small>AMOUNT</small>
            <input placeholder="0" id="LINK" type="number" class="amount-selector-input" v-model="LINK" />
          </div>
        </div>
      </div>
      <!-- <small class="swap-element-name">/TO</small> -->

      <div class="confirm-button-container">
        <div class="confirm-text">SWAP {{ ETH }} ETH for {{ LINK }} LINK</div>
        <button class="confirm-button" v-on:click="$emit('buttonPressed', {ETH, LINK})">
          SWAP
        </button>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      ETH: null,
      LINK: null,
      ETHSelected: true,
    };
  },
  props: ["isProcessing", "LINKETH"],
  emits: ["buttonPressed"],
  watch: {
    ETH: function () {
      if (document.activeElement.id === "ETH") {
        this.LINK = ((1 / this.LINKETH) * this.ETH).toFixed(5);
      }
    },
    LINK: function () {
      if (document.activeElement.id === "LINK") {
        this.ETH = (this.LINKETH * this.LINK).toFixed(5);
      }
    },
  },
};
</script>

<style>
.swap-container {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  top: -20px;
}

.swap-element-name {
  color: white;
}

.swap-element {
  background: black;
  border: 1px solid #212121;
  color: white;
  text-align: left;
  width: 30%;
  min-height: 160px;
  margin: 48px 0;
}

.swap-element-top {
  width: 100%;
  background: #121212;
  height: 80px;
  display: flex;
  justify-content: space-between;
}

.swap-token-selector,
.swap-network-selector,
.swap-amount-selector {
  padding: 4px 12px;
  color: grey;
}

.swap-network-selector {
  border-left: 1px solid black;
}

.swap-amount-selector {
  background: black;
}

.swap-token-list {
  background: #000;
  border: 1px solid #212121;
  color: white;
}

.token-image {
  width: 32px;
  border-radius: 50%;
}

.token-name,
.network-name {
  color: white;
  margin: 0 8px 0 8px;
}

.token-selector-input,
.network-selector-input,
.amount-selector-input {
  background: #212121;
  display: flex;
  width: fit-content;
  padding: 4px;
  border: 1px solid #282828;
  align-items: center;
  color: white;
  font-size: 1.4em;
}

.amount-selector-input {
  background: black;
  border: unset;
  border-bottom: 1px solid #212121;
  width: 30%;
}

.swap-button {
  color: rgb(175, 175, 175);
  font-size: 2em;
}

.confirm-button-container {
  position: fixed;
  display: block;
  bottom: 0;
  margin-bottom: 84px;
}

.confirm-text {
  color: white;
  margin-bottom: 12px;
}

.confirm-button {
  width: 30vw;
  height: 64px;
  background: white;
  font-size: 1.4em;
  font-weight: 600;
  border: unset;
  border-radius: 8px;
  transition: 100ms ease all;
}

.confirm-button:hover {
  background: lightgrey;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

</style>