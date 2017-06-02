<template>
  <div class="cards">
    <h1>{{ msg }}</h1>
    <button class="btn btn-success" @click="getDeck(), getCards(deckData)">Start/Shuffle!</button>
    <hr/>
    <img v-for="card in cardList" :src="card.image"></img>
  </div>
</template>

<script>
export default {
  name: 'cards',
  data () {
    return {
      msg: 'Click "Start/Shuffle to display and shuffle deck"',
      deck: '',
      cardList: ''
    }
  },
  computed: {
    cardData () {
      return this.cardList
    },
    deckData () {
      return this.deck
    }
  },
  methods: {
    getDeck: function () {
      return fetch('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1')
        .then(res => res.json())
        .then(json => {
          this.deck = json
          return Promise.resolve(json)
        })
        .catch(err => Promise.reject(err))
    },
    getCards: function (deck) {
      return fetch(`https://deckofcardsapi.com/api/deck/${deck.deck_id}/draw/?count=52`)
        .then(response => response.json())
        .then((json) => {
          this.cardList = json.cards.slice()
          return Promise.resolve(json)
        })
        .catch(err => Promise.reject(err))
    },
    shuffle: function (deck) {
      return fetch('https://deckofcardsapi.com/api/deck/' + deck.deck_id + '/shuffle/')
       .then(response => response.json())
       .then(json => {
         this.deck = json
         return Promise.resolve(json)
       })
      .catch(err => Promise.reject(err))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

</style>
