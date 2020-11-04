<template>
  <div class="container">
    <div>
      <logo />
      <h2 class="subtitle">
        Enter your address
      </h2>
      <AddressInputContainer />
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import AddressInputContainer from '~/components/AddressInputContainer.vue'

export default {
  components: {
    Logo,
    AddressInputContainer
  }
}
</script>

<style>
html, body {
  background-color: #000;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.tooltip .tooltip-inner {
  background: #fff;
  color: #000;
  font-size: 12px;
  border-radius: 16px;
  padding: 5px 10px 4px;
  z-index: 20px;
  width: 150px;
  text-align-last: center;
}

.tooltip .popover-inner {
  background: #fff;
  color: #000;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, .1);
  z-index: 20px;
}

.tooltip .popover-arrow {
  border-color: #fff;
}
</style>
