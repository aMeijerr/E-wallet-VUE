<template>
  <div class="homeview">
    <h1>E-WALLET</h1>
    <div class="activeCard" v-if="cardInfo.length">
      <CardItem :cardInfo="activatedCard" />
      <button
        v-if="activatedCard.active"
        @click="removeActive"
        class="removeActive"
      >
        Remove this card
      </button>
    </div>
    <h2>{{ welcomeText }}</h2>
    <CardList :cardInfo="cardInfo" @isActive="active" />
    <button @click="changeView" class="generalBtn">ADD NEW CARD</button>
  </div>
</template>

<script>
import CardItem from "../components/CardItem";
import CardList from "../components/CardList";
export default {
  components: { CardList, CardItem },
  props: {
    cardInfo: Array,
  },
  data() {
    return {
      activatedCard: {},
    };
  },
  methods: {
    changeView() {
      this.$emit("sendCreate");
    },
    active(card) {
      this.activatedCard = card;
    },
    removeActive() {
      this.$emit("remove", this.activatedCard);
      this.activatedCard = {};
    },
  },
  computed: {
    welcomeText() {
      if (this.cardInfo.length) {
        return "";
      }
      return "Welcome to your E-Wallet, please add some cards!";
    },
  },
};
</script>

<style lang="scss" scoped>
.removeActive {
  border: none;
  background-color: transparent;
  font-size: 0.5rem;
  color: red;
}
.generalBtn {
  margin-top: 12rem;
  margin-bottom: 3rem;
  padding: 1rem;
  font-weight: 600;
  border-radius: 5px;
  background-color: white;
  width: 290px;
}
.generalBtn:hover {
  background-color: black;
  color: white;
}

.activeCard {
  margin-bottom: 1rem;
}
</style>
