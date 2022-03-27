<template>
  <div
    @click="$emit('activeCard')"
    class="card-template"
    :class="cardInfo.vendor"
  >
    <div class="wifi-chip">
      <img src="../assets/wifi.svg" class="wifi" />
      <img src="../assets/chip.svg" class="chip" />
    </div>
    <div class="vendorIcon">
      <img
        v-if="cardInfo.vendor"
        :src="require(`../assets/${cardInfo.vendor}.svg`)"
        alt="currency"
      />
    </div>
    <div class="card-number">
      <h4>{{ cardInfo.cardNumber }}</h4>
    </div>
    <div class="cardholder labels">
      <label>CARDHOLDER NAME</label>
    </div>
    <div class="name">
      <p>{{ cardInfo.cardholder }}</p>
    </div>
    <div class="valid-label labels">
      <label>VALID THRU</label>
    </div>
    <div class="valid-dates">
      <p v-if="cardInfo.expireMonth == null && cardInfo.expireYear == null">
        MM/YY
      </p>
      <p v-else>{{ cardInfo.expireMonth }}/{{ cardInfo.expireYear }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cardInfo: Object,
    index: Number,
  },
};
</script>

<style lang="scss">
html {
  height: 100vh;
}
.name {
  text-transform: uppercase;
}
</style>
