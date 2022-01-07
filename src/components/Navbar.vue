<template>
  <div class="navbar-container">
    <div class="navbar-content">
      <div class="navbar-title">HYPERSPEED</div>
      <div class="navbar-buttons">
        <div class="navbar-button"><b>/SWAP</b></div>
        <div class="navbar-button">/ABOUT</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>
.navbar-container {
  left: 0;
  top: 0;
  position: absolute;
  height: 64px;
  border-bottom: 1px solid #212121;
  width: 100%;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(4px);
}

.navbar-content {
  margin: 0 auto;
  width: 60%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar-title {
  color: white;
  font-size: 2em;
  font-weight: 600;
}

.navbar-buttons {
  flex-grow: 1;
  display: flex;
  color: white;
  margin-left: 72px;
}

.navbar-button {
  margin: 0 12px;
  padding: 12px;
}
</style>