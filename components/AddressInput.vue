<template>
  <div class="flex items-end form go-bottom">
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
    <div class="form-control ml-8">
      <button
        v-if="showTooltip"
        v-tooltip.bottom="'Start typing address to get suggestions from Google'"
        class="h-8 text-white border border-white btn-tooltip"
      >
        ?
      </button>
      <input id="address" ref="searchTextField" type="text" placeholder="Address" autocomplete="off">
      <label for="address">Address</label>
      <button
        v-if="selectedPlace && selectedPlace.formatted_address.length"
        @click="clearAddress"
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
      <input
        id="num"
        @blur="addAddressNumber"
        v-model="addressNumber"
        type="text"
        placeholder="No."
        class="add-num"
      >
      <label for="num">N<sup>o</sup></label>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-undef */

export default {
  props: {
    showTooltip: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      type: 'address',
      defaultBounds: '',
      autocomplete: '',
      showNumber: false,
      selectedPlace: '',
      addressNumber: '',
      options: ''
    }
  },
  watch: {
    type (val) {
      this.initializeInput(this.$refs.searchTextField, this.options)
    }
  },
  mounted () {
    const input = this.$refs.searchTextField
    const defaultBounds = new google.maps.LatLngBounds(
      new google.maps.LatLng(-33.8902, 151.1759),
      new google.maps.LatLng(-33.8474, 151.2631))
    this.options = {
      bounds: defaultBounds,
      types: [this.type]
    }
    this.initializeInput(input, this.options)
  },
  methods: {
    initializeInput (input, options) {
      this.autocomplete = new google.maps.places.Autocomplete(
        input, options
      )
      google.maps.event.addListener(this.autocomplete, 'place_changed', () => {
        this.showNumber = true
        this.selectedPlace = this.autocomplete.getPlace()
      })
    },
    addAddressNumber () {
      const addNum = this.addressNumber.length ? `${this.addressNumber}, ` : ''
      this.$refs.searchTextField.value = `${addNum}${this.selectedPlace.formatted_address}`
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
  padding: 12px;
}
.form-control {
  position: relative;
  padding-left: 30px;
}

.form-control .btn-tooltip {
  position: absolute;
  bottom: -10px;
  left: 0;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  line-height: 0.7;
  z-index: 20;
  font-size: 12px;
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

.form-control input.add-num {
  width: 64px;
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
  margin-left: 44%;
}
</style>
