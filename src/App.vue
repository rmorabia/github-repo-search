<template>
  <div id="app">
    <Nav />
    <Form
      @submit="axios"
      :loading = loading
    />
    <p v-if="loading"><i class="fas fa-spinner"></i> Loading... </p>
    <p v-if="noResults">No results match your search. Try again!</p>
    <p v-if="err">There was an error with your search request. Try again.</p>
    <p v-if="response === false && loading === false">Please enter query and click SEARCH button above, results appear here.</p>
    <p v-if="response">SEARCH results:</p>
    <Results
      v-for="item in response"
      :response = item
    />
    <Footer />
  </div>
</template>

<script>
import Form from './components/Form'
import Results from './components/Results'
import Nav from './components/Nav'
import Footer from './components/Footer'

const axios = require('axios')

export default {
  name: 'app',
  components: {
    Form,
    Results,
    Nav,
    Footer
  },
  data: () => {
    return {
      loading: false,
      noResults: false,
      err: false,
      response: false
    }
  },
  methods: {
    axios (e) {
      const self = this
      const sanitizedText = e.text.replace(' ', '+')
      this.loading = true
      axios
        .get('https://api.github.com/search/repositories?q=' + sanitizedText + '+stars:' + e.stars + '+license:' + e.license + '+fork:' + e.forked)
        .then(function (response) {
          self.loading = false
          if (response.data.total_count === 0) {
            self.noResults = true
          } else {
            self.noResults = false
          }
          self.response = response.data.items
        })
        .catch(function (error) {
          self.err = true
        })
    }
  }
}
</script>

<style>
body {
  min-height: 100%;
  position: relative;
  padding-bottom: 60px;
}

#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000;
}

.container {
  max-width: 1600px;
  margin: 0 auto;
}

.content-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

a {
  color: #3f87c6;
}

footer {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  height: 60px;
}

.row {
  margin: 0;
}

p {
  margin: 20px;
  text-align: center;
}
</style>
