<template>
  <div class="container">
    <div class="window">
      <div class="mb-20">
        <div>Input message here</div>
        <input type="text" v-model="input">
      </div>
      <div>Text shown here</div>
      <p class="border-bottom">{{ inputLower }}</p>
    </div>
  </div>
  <div class="container pt-40">
    <div class="currency-container">
      <div class="d-flex mb-20">
        <div class="mr-20">
          <div class="white-text">Amount</div>
          <input class="amount" type="number" ref="enteredValue">
        </div>
        <div class="mr-20">
          <div class="white-text">From</div>
          <select name="fromCurr" v-model="fromCurr" value="MYR">
            <option value="USD">USD</option>
            <option value="EUR">EUR</option>
            <option value="MYR">MYR</option>
          </select>
        </div>
        <img src="" alt="">
        <div class="mr-20">
          <div class="white-text">To</div>
          <select name="toCurr" v-model="toCurr" value="USD">
            <option value="USD">USD</option>
            <option value="EUR">EUR</option>
            <option value="MYR">MYR</option>
          </select>
        </div>
        <button class="convert" @click="convert">Convert</button>
      </div>
      <div class="show-results" v-if="prices && !isInvalid">
        <div class="primary" v-for="price in prices" :key="price.id">
          {{ amount }} {{ price.fr }} = {{ totalAmount(price) }} {{ price.to }}
        </div>
      </div>
      <div class="white-text" v-else>
        Nothing right now...
      </div>
    </div>
  </div>
</template>

<script>

//http://free.currconv.com/api/v7/convert?apiKey=a91b940d0fc2ad4a84d8&q=USD_MYR
//apiKey=a91b940d0fc2ad4a84d8
export default {
  name: 'App',
  data() {
    return {
      input: '',
      amount: 0,
      fromCurr: 'MYR',
      toCurr: 'USD',
      total: 0,
      prices: null,
      isInvalid: false
    }
  },
  methods: {
    convert() {
      fetch('http://free.currconv.com/api/v7/convert?apiKey=a91b940d0fc2ad4a84d8&q=' + this.fromCurr + '_' + this.toCurr)
      .then((res) => {
        if(res.ok) {
          return res.json()
        }else {
          throw Error('error, cant return json')
        }
      }).then(price => {
        return this.prices = price.results
      }).catch((err) => {
        console.log(err.message)
      })
      
      this.amount = this.$refs.enteredValue.value

      if(this.amount == 0) {
        this.isInvalid = true
      }else {
        this.isInvalid = false
      }
    },
    totalAmount(price) {
      this.total = this.amount * price.val
      return (this.total.toFixed(3))
    }
  },
  computed: {
    inputLower() {
      return this.input.toLowerCase()
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
select {
  width: 160px;
  height: 30px;
  box-sizing: border-box;
}
.white-text {
  color: #fff;
}
.mb-20 {
  margin-bottom: 20px;
}
.mr-20 {
  margin-right: 20px;
}
.pt-40 {
  padding-top: 40px;
}
.d-flex {
  display: flex;
}
.border-bottom{
  border-bottom: 1px solid #eee;
}
.container {
  width: 768px;
  margin: 0 auto;
}
.window {
  box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
  -webkit-box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
  -moz-box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
  border-radius: 5px;
  padding: 20px;
}
.currency-container {
  background-color: #3498db;
  padding: 20px;
  border-radius: 6px;
  box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
  -webkit-box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
  -moz-box-shadow: 5px 7px 5px 0px rgba(0,0,0,0.25);
}
.show-results {
  color: #fff;
}
.amount {
  box-sizing: border-box;
  width: 160px;
  height: 30px;
}
.convert {
  appearance: none;
  outline: none;
  background-color: #2980b9;
  border: none;
  font-weight: bold;
  align-self: flex-end;
  width: 100px;
  height: 30px;
  padding: 8px 15px;
  border-radius: 50px;
  color: #fff;
  cursor: pointer;
}
.convert:hover {
  background-color: #e67e22;
}
</style>
