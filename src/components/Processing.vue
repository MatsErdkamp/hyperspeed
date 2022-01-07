<template>
  <div class="processing-container">
    <div v-if="isCompleted == true" class="processing-content-confirmed">
      <h1 class="processing-title">CONFIRMED</h1>
      You bought {{ LINK }} LINK!
    </div>
    <div v-else class="processing-content">
      <h1 class="processing-title">PROCESSING</h1>
      {{ ETH }} ETH <small style="color:lightgrey">for</small> {{ LINK }} LINK
    </div>
  </div>
</template>

<script>
export default {
  props: ["isCompleted", "LINK", "ETH"],
};
</script>

<style>
.processing-container {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  top: 0;
}

.processing-content {
  text-align: center;
  color: white;
  font-weight: 500;
  margin-bottom: 60px;
  max-width: 260px;
}

.processing-content-confirmed {
  text-align: center;
  color: black;
  font-weight: 500;
  margin-bottom: 60px;
}
</style>