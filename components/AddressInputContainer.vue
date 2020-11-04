<template>
  <div>
    <div v-for="(item, index) in addresses" :key="item.id">
      <div class="flex">
        <AddressInput @add-new-address="addAddress" />
        <button v-if="index !== 0" @click="removeAddress(index)" class="text-red-500">
          x
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AddressInput from '~/components/AddressInput.vue'

export default {
  components: {
    AddressInput
  },
  data () {
    return {
      addresses: [
        {
          id: Date.now()
        }
      ]
    }
  },
  methods: {
    addAddress () {
      this.addresses.push({ id: Date.now() })
    },
    removeAddress (index) {
      this.addresses.splice(index, 1)
    }
  }
}
</script>

<style scoped>

</style>
