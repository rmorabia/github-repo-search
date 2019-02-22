<template>
  <div class="content-container">
    <h1>GitHub Repository Search</h1>
    <form @submit.prevent="submit">
      <div class="row">
        <div class="col-sm-6">
          <label for="text">Text</label>
          <input type="text" name="text" id="text" placeholder="react" v-model="form.text" v-validate="'required|alpha_spaces|max:255'"
            :disabled="loading" required>
          <div>{{ errors.first('text') }}</div>
        </div>
        <div class="col-sm-6">
          <label for="stars">Stars</label>
          <input type="text" name="stars" id="stars" placeholder="<1000" v-model="form.stars" :disabled="loading" required v-validate="{
            required: true,
            max: 255,
            regex: /^(([<>]=?))?(\d*)$/
          }">
          <div>{{ errors.first('stars') }}</div>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-6">
          <label for="license">License</label>
          <select name="license" v-model="form.license" v-validate="'required'" :disabled="loading" required>
            <option value="mit">MIT</option>
            <option value="isc">ISC</option>
            <option value="apache-2.0">Apache license 2.0</option>
            <option value="gpl">GNU General Public License</option>
          </select>
        </div>
        <div class="col-sm-6">
          <div class="check-label"></div>
          <div>
            <input id="checkbox" type="checkbox" name="forked" value="forked" v-model="form.forked" :disabled="loading">
            Include Forked
          </div>
        </div>
      </div>
      <div class="row submit-row">
        <input type="submit" value="Search" :disabled="loading">
      </div>
      <div class="error" v-if="err">There was an error with your submission, please try again.</div>

    </form>
    <hr>

  </div>
</template>

<script>
export default {
  name: 'Form',
  props: {
    loading: Boolean
  },
  data: () => {
    return {
      form: {
        text: '',
        stars: '',
        license: '',
        forked: false
      },
      err: false
    }
  },
  methods: {
    submit () {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.$emit('submit', this.form)

          // Param updating
          const params = new URLSearchParams(location.search)
          params.set('text', this.form.text)
          params.set('stars', this.form.stars)
          params.set('license', this.form.license)
          params.set('forked', this.form.forked)

          window.history.pushState('', 'Update params', '?' + params.toString())
        } else {
          this.err = true
        }
      })
    }
  },
  mounted () {
    const params = new URLSearchParams(location.search)

    let forked = params.get('forked')
    if (forked === 'false' | 'False') {
      forked = false
    }

    // Update form fields based on initial query params
    if (params.get('text')) {
      this.form.text = params.get('text')
    }
    if (params.get('stars')) {
      this.form.stars = params.get('stars')
    }
    if (params.get('license')) {
      this.form.license = params.get('license')
    }
    if (params.get('forked')) {
      this.form.forked = forked
    }

    // Update the query URL on each click
    const self = this
    document.addEventListener('click', function (event) {
      params.set('text', self.form.text)
      params.set('stars', self.form.stars)
      params.set('license', self.form.license)
      params.set('forked', self.form.forked)

      window.history.pushState('', 'Update params', '?' + params.toString())
    })
  }
}
</script>

<style scoped>

@media only screen and (max-width:48em) {
  .row {
    flex-direction: column;
  }
}

.submit-row {
  justify-content: center;
  padding: 50px 0;
}

input[type="text"] {
  border: 1px solid #000;
  padding: 10px;
  border-radius: 0;
  -webkit-appearance: none;
  width: 100%;
  box-sizing: border-box;
}

select {
  -webkit-appearance: none;
  background: #fff no-repeat 100%;
  border-radius: 0;
  -webkit-transition: all .2s ease;
  border: 1px solid #000;
  display: block;
  font-family: inherit;
  width: 100%;
  padding: 10px;
  transition: all .2s ease;
}

.check-label {
  height: 38px;
}

input[type="checkbox"] {
  width: 1.6rem;
  height: 1.6rem;
  margin: 3px;
}

input[type="submit"] {
  background-color: #3f87c6;
  color: #fff;
  padding: 10px 30px;
  font-weight: 600;
  text-transform: uppercase;
}

h1 {
  text-align: center;
}

label {
  display: block;
  padding: 10px 0;
}

.error {
  text-align: center;
}

</style>
