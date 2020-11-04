<template>
  <div class="flex form go-bottom">
    <select v-model="type" class="select">
      <option value="address">
        Residential
      </option>
      <option value="domicile">
        Domicile
      </option>
      <option value="legal">
        Legal
      </option>
      <option value="operational">
        Operational
      </option>
    </select>
    <!-- <div class="bg-white p-40">
      <button v-tooltip="'You have new messages.'" class="has-tooltip v-tooltip-open">ghghg</button>
    </div> -->
    <div class="form-control">
      <input id="address" ref="searchTextField" type="text" placeholder="Address" autocomplete="off">
      <label for="address">Address</label>
      <button
        v-if="selectedPlace && selectedPlace.formatted_address.length"
        @click="clearAddress"
        v-tooltip="'You have new messages.'"
        class="btn-clear"
      >
        x
      </button>
      <button
        v-if="selectedPlace && selectedPlace.formatted_address.length"
        @click="$emit('add-new-address')"
        class="btn-new"
      >
        +
      </button>
    </div>
    <div v-if="showNumber" class="form-control">
      <input id="num" @blur="addAddressNumber" v-model="addressNumber" type="text" placeholder="Number">
      <label for="num">N<sup>o</sup></label>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-undef */

export default {
  data () {
    return {
      type: 'address',
      defaultBounds: '',
      autocomplete: '',
      showNumber: false,
      selectedPlace: '',
      addressNumber: ''
    }
  },
  mounted () {
    const input = this.$refs.searchTextField
    const defaultBounds = new google.maps.LatLngBounds(
      new google.maps.LatLng(-33.8902, 151.1759),
      new google.maps.LatLng(-33.8474, 151.2631))
    const options = {
      bounds: defaultBounds,
      types: ['address']
    }
    this.autocomplete = new google.maps.places.Autocomplete(
      input, options
    )
    google.maps.event.addListener(this.autocomplete, 'place_changed', (e) => {
      this.showNumber = true
      this.selectedPlace = this.autocomplete.getPlace()
    })
  },
  methods: {
    addAddressNumber () {
      this.$refs.searchTextField.value = `${this.addressNumber}, ${this.selectedPlace.formatted_address}`
      this.showNumber = false
      this.addressNumber = ''
    },
    clearAddress () {
      this.showNumber = false
      this.$refs.searchTextField.value = ''
      this.selectedPlace = ''
    }
  }
}
</script>

<style scoped>
select:focus, input:focus {
  outline: none;
  box-shadow: none;
}
.select {
  background: transparent;
  color: #fff;
  border-bottom: 1px solid #fff;
}
.form-control {
  position: relative;
  margin: 0 10px;
}

.form-control .btn-clear {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 10;
  background-color: #fff;
  color: #000;
  font-size: 10px;
  border-radius: 50%;
  padding: 0 4px;
  font-weight: bold;
  line-height: 1.5;
}

.form-control .btn-new {
  position: absolute;
  bottom: 0;
  right: 0;
  z-index: 10;
  color: #fff;
  background-color: black;
  font-weight: bold;
  margin-bottom: -10px;
  padding: 0 5px;
}

.form-control input {
  width: 100%;
  border-bottom: 2px solid gray;
  background: none;
  position: relative;
  top: 0;
  left: 0;
  z-index: 1;
  padding: 8px 12px;
  outline: 0;
  color: #fff;
}

.form-control input:valid {
  background: #000;
}

.form-control input:focus + label {
  color: #fff;
  font-size: 70%;
  padding: 1px 6px;
  z-index: 2;
  text-transform: uppercase;
}

.form-control > label {
  text-transform: uppercase;
  transition:
    background 0.2s,
    color 0.2s,
    top 0.2s,
    bottom 0.2s,
    right 0.2s,
    left 0.2s;
  position: absolute;
  background-color: #000;
  color: #999;
  padding: 7px 6px;
}

.form.go-bottom input {
  padding: 12px;
}
.form.go-bottom label {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  width: 35%;
  margin: 0 auto;
  background-color: #000;
}
.form.go-bottom input:focus + label {
  top: 100%;
  margin-top: -10px;
}
.tooltip .popover {
  color: #f9f9f9;
}

.tooltip .popover-inner {
  background: #fff;
  color: #fff;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, .1);
}

.tooltip .popover-arrow {
  border-color: #fff;
}
</style>
