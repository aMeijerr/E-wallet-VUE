<template>
  <div>
    <div class="showList">
      <CardItem
        v-for="(card, index) in cardInfo"
        :key="index"
        @activeCard="$emit('isActive', card)"
        :cardInfo="card"
      />
    </div>
  </div>
</template>

<script>
import CardItem from "./CardItem";
export default {
  components: { CardItem },
  props: {
    cardInfo: Array,
  },
};
</script>

<style lang="scss">
.showList {
  display: grid;
  grid-auto-rows: 3.5rem;
}
</style>
