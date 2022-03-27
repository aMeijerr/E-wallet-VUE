<template>
<div class="createView">
  <h1>ADD A NEW BANK CARD</h1>
  <CardItem :cardInfo="cardInfo"/>
    <form
  v-on:submit.prevent="checkForm"
  method="post"
  novalidate="true"
>

  <p v-if="errors.length" class="error-container">
    <b>Please correct the following error(s):</b>
    <ul>
      <li 
      v-for="error in errors" v-bind:key="error">{{ error }}
      </li>
    </ul>
  </p>

  <p>
    <label for="cardNumber">CARD NUMBER</label>
    <input
      id="cardNumber"
      :placeholder="placeholder_cardNum"
      oninput="if(this.value.length == 4 || this.value.length == 9 || this.value.length == 14) this.value += ' ';"
      v-on:keypress="checkCard($event)"
      v-model="cardInfo.cardNumber"
      type="text"
      name="cardNumber"
      maxlength="19"
    >
  </p>
  <p>
    <label for="cardholder">CARDHOLDER NAME</label>
    <input
      id="cardholder"
      :placeholder="placeholder_cardName"
      v-model="cardInfo.cardholder"
      type="text"
      name="cardholder"
      v-on:keypress="isLetter($event)"
    >
  </p>
<div class="dateDropdown">
  <p>
    <label for="expireMonth">MONTH</label>
    <select name = "dropdown" v-model="cardInfo.expireMonth">
        <option v-for="n in 12" :key="n">
          {{(n > 9) ? n : '0' + n}}</option>
         </select>
  </p>

   <p>
    <label for="expireYear">YEAR</label>
    <select name = "dropdown" v-model="cardInfo.expireYear">
        <option v-for="n in 5" :key="n" :value="n + 21">{{n + 21}}</option>
         </select>
  </p>
  </div>

  <p>
    <label for="vendor">VENDOR</label>
    <select name = "dropdown" id="vend" v-model="cardInfo.vendor">
        <option value="bitcoin">Bitcoin INC</option>
        <option value="ninja">Ninja bank</option>
        <option value="blockchain">Block chain INC</option>
        <option value="evil">Evil corp</option>
         </select>
  </p>

  <p>
    <button class="submit"
    >ADD CARD</button>
  </p>
  <button @click="changeView" class="clear">Go back</button>

</form>
</div>
</template>

<script>
import CardItem from '../components/CardItem'
export default {
  components: { CardItem
  },

  data(){
        return{
          placeholder_cardName: "Firstname Lastname",
          placeholder_cardNum: "XXXX XXXX XXXX XXXX",
          errors: [],
          currentCards: [],
          cardInfo:
            {cardNumber: null,
            cardholder: null,
            expireMonth: null,
            expireYear: null,
            vendor: null,
            active: true}
        } 
    },
    methods: {
      changeView() {
      this.$emit("sendHome");
},
    checkForm: function () {
      this.errors = [];
      if (!this.cardInfo.cardNumber) {
        this.errors.push("Card number required.");
      }
      if (!this.cardInfo.cardholder) {
        this.errors.push("Name required.");
      }
      if (!this.cardInfo.expireMonth) {
        this.errors.push("Month required.");
      }
      if (!this.cardInfo.expireYear) {
        this.errors.push("Year required.");
      }
      if (!this.cardInfo.vendor) {
        this.errors.push("Vendor required.");
      }
      for(let i = 0; i < this.currentCards.length; i++){
      if(this.cardInfo.cardNumber == this.currentCards[i]){
        this.errors.push("You have already registered this card! Please check your cardnumber.");
        }
        }
      if (!this.errors.length) {
        this.$emit("sendForm", {...this.cardInfo});
        this.currentCards.push(this.cardInfo.cardNumber);
      }
  },
  isLetter(e) {
  let char = String.fromCharCode(e.keyCode);
  if(/^[A-Za-z\s]+$/.test(char)) return true;
  else e.preventDefault();
  
},
   checkCard(e){
     let num = String.fromCharCode(e.keyCode);
  if(/^[0-9]+$/.test(num)) return true;
  else e.preventDefault();
  if(e.keyCode === 32){
    e.preventDefault();
  }
},
    },
}

</script>

<style lang="scss" scoped>
//FORM CSS
form{
  margin-top: 1rem;
  padding: 0;
  width: 290px;
}
ul{
  font-size: 0.6rem;
  padding: 2px;
  margin: 1rem;
}
li{
  color: red;
  list-style: none;
}
form p{
  margin: 0 0 0.5rem 0;
}
form label{
  display: flex;
  flex-direction: row;
  font-size: 0.5rem;
}
.createView{
display: flex;
  flex-direction: column;
  align-items: center;
  margin: auto;
  max-width: 290px;
  }

.dateDropdown{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.dateDropdown select {
  padding: 1px 2px;
  width: 140px;
}
.submit{
  margin-top: 0.5rem;
  padding: 0.5rem;
  font-weight: 600;
  border-radius: 5px;
  background-color: white;
  width: 290px;
}
.submit:hover{
    background-color: black;
    color: white;
  }

#cardNumber, #cardholder{
  width: 280px;
}
#vend {
  width: 290px;
  padding: 1px 2px;
}

.error-container{
  width: 290px;
}
</style>
