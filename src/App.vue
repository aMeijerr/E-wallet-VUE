<template>
  <div id="app">
    <div class="deleteModal" v-if="confirmDelete">
      <p>Deleting all cards, are you sure?</p>
      <button @click="setDelete">Yes</button>
      <button @click="clearStorage">No</button>
    </div>
    <div class="singleDelete" v-if="singleDelete">
      <p>Your card was removed!</p>
      <button @click="singleDelete = false">Thanks</button>
    </div>
    <Home
      v-if="currentView == 'home'"
      @sendCreate="currentView = 'create'"
      :cardInfo="cardInfo"
      @remove="removeActive"
    />

    <Create
      v-else-if="currentView == 'create'"
      @sendHome="currentView = 'home'"
      @sendForm="register"
      :cardInfo="cardInfo"
    />
    <button v-if="cardInfo.length" @click="confirmDelete = true" class="clear">
      CLEAR ALL CARDS
    </button>
  </div>
</template>

<script>
import Home from "./views/Home";
import Create from "./views/Create";
export default {
  components: { Home, Create },
  name: "App",
  data() {
    return {
      singleDelete: false,
      confirmDelete: false,
      currentView: "home",
      cardInfo: [],
    };
  },
  methods: {
    register(formData) {
      this.cardInfo.push(formData);
      localStorage.setItem("cardInfo", JSON.stringify(this.cardInfo));
    },
    clearStorage() {
      this.confirmDelete = false;
    },
    setDelete() {
      this.confirmDelete = false;
      localStorage.clear();
      this.cardInfo = [];
    },
    removeActive(removeActive) {
      for (let i = 0; i < this.cardInfo.length; i++) {
        if (removeActive == this.cardInfo[i]) {
          this.cardInfo.splice(i, 1);
        }
        this.singleDelete = true;
      }
    },
  },
  created() {
    if (localStorage.cardInfo) {
      this.cardInfo = JSON.parse(localStorage.getItem("cardInfo"));
    }
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Oswald:wght@300&family=Source+Sans+Pro:wght@900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Oswald:wght@300&family=PT+Mono&family=Source+Sans+Pro:wght@900&display=swap");
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  text-align: center;
  color: #2c3e50;
  width: 290px;
  padding: 0;
  margin: auto;
  overflow: hidden;
}
html::-webkit-scrollbar {
  display: none;
}
.deleteModal,
.singleDelete {
  position: absolute;
  top: 20rem;
  z-index: 10;
  background-color: white;
  color: red;
  padding: 2px;
  width: 160px;
  height: 90px;
  border: 1px solid black;
}
.deleteModal button,
.singleDelete button {
  margin: 5px;
  font-weight: 600;
  border-radius: 5px;
  background-color: white;

  &:hover {
    background-color: black;
    color: white;
  }
}
.clear {
  // padding: 1rem;
  margin-bottom: 5rem;
  font-weight: 600;
  border-radius: 5px;
  border: 1px red solid;
  background-color: white;
}
.clear:hover {
  background-color: black;
  color: white;
}
h1 {
  font-family: "Source Sans Pro", sans-serif;
}
label,
button,
p,
h2,
h3,
h4,
h5,
h6 {
  font-family: "PT Mono", monospace;
}
//CARD CSS BELOW ------
img {
  width: 2rem;
}
label {
  font-size: 0.6rem;
}
p {
  margin-left: 0.5rem;
  font-size: 0.8rem;
}
.card-template.ninja {
  background-color: #222222;
  color: white;
}
.card-template.bitcoin {
  background-color: #ffb84d;
  color: black;
}
.card-template.blockchain {
  background-color: #8b58f9;
  color: white;
}
.card-template.evil {
  background-color: #f33355;
  color: white;
}
.card-template {
  height: 180px;
  border-radius: 0.5rem;
  padding: 0.5rem;
  margin: 0;
  display: grid;
  background-color: lightgray;
  grid-template-columns: 55px 55px 55px 55px 55px;
  grid-template-rows: 35px 35px 35px 35px 35px;
  grid-template-areas:
    "wifi header header header vendor"
    "chip header header header vendor"
    "main main main main main"
    "cardhold cardhold footer valid valid"
    "name name name dates dates";
  box-shadow: inset -12px -8px 50px #464646;
}
.labels {
  margin-top: 5px;
}
.wifi-chip {
  margin-top: 0.5rem;
}
.wifi {
  grid-area: wifi;
}
.chip {
  grid-area: chip;
  background-color: lightgray;
  border-radius: 3px;
}
.vendorIcon {
  grid-area: vendor;
  justify-self: center;
  margin-top: 0.5rem;
}
.card-number {
  grid-area: main;
  font-size: 1.23rem;
}

.cardholder {
  grid-area: cardhold;
  margin-top: 1.5rem;
}
.name {
  grid-area: name;
  margin-left: 0.3rem;
  justify-self: start;
}
.valid-label {
  grid-area: valid;
  margin-top: 1.5rem;
}
.valid-dates {
  grid-area: dates;
  margin-left: 0.8rem;
}
</style>
