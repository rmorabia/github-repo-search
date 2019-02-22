<template>
  <div>
    <div class="row results-block">
      <div class="col-sm-8">
        <div class="results">
          <div class="row">
            <div class="col-xs-8">
              <h1><a :href="response.html_url">{{ response.full_name }}</a></h1>
            </div>
            <div class="col-xs-4">
              <div v-if="response.fork" class="forked">forked</div>
            </div>
          </div>
          <p>{{ response.description }}</p>
        </div>
      </div>
      <div class="col-sm-2">
        <div class="results-small">
          <div class="row">
            <div class="results-label col-sm-12 col-lg-6">Stars: </div>
            <div class="bold col-sm-12 col-lg-6"> {{ response.stargazers_count }} </div>
          </div>
        </div>
      </div>
      <div class="col-sm-2">
        <div class="results-small">
          <div class="row">
            <div class="results-label col-sm-12 col-lg-6">License: </div>
            <div class="bold col-sm-12 col-lg-6"> {{ response.license.name }} </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Results',
  props: {
    response: Object
  }
}
</script>

<style scoped>

.row {
  margin: 10px 0;
}

.results-block {
  margin: 20px;
}

.results {
  padding: 10px;
}

.results-small {
  text-align: center;
  padding: 17px;
}

.results-label {
  padding: 10px;
}

.bold {
  font-weight: 600;
  padding: 10px;
}

.forked {
  margin: 10px;
  background-color: #3f87c6;
  color: #fff;
  padding: 10px;
  text-align: center;
}

.col-sm-8,
.col-sm-2 {
  border: 1px solid #ccc;
  width: 100%;
}

h1 {
  font-size: 22px;
  color: #3f87c6;
  word-break: break-word;
}

p {
  text-align: left;
  margin: 10px;
}

a {
  text-decoration: none;
}
</style>
