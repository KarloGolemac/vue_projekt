<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info" id="nav">
      <b-navbar-brand href="#">NUMBER FACTS</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search" v-model="searchText"></b-form-input>

            <b-form-group id="radio">
              <b-form-radio-group
                v-model="selected"
                :options="options"
                plain
                stacked
                name="plain-stacked"
              ></b-form-radio-group>
            </b-form-group>

            <!-- search the number that the user typed in -->
            <b-button size="sm" class="my-2 my-sm-0"
            @click="dohvatiPodatke(searchText, selected)">Search</b-button>

            <!-- search for random number based on radio button -->
            <b-button variant="success" @click="dohvatiPodatke(randomNumber(), selected)">RANDOM</b-button>
          </b-nav-form>

        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <div class="content">

      <h2>TRIVIA</h2>
      <ul>
        <li v-for="item in trivia" :key="item.message"> {{ item }} </li>
      </ul>

      <hr>

      <h2>MATH</h2>
      <ul>
        <li v-for="item in math" :key="item.message"> {{ item }} </li>
      </ul>

      <hr>

      <h2>YEAR</h2>
      <ul>
        <li v-for="item in year" :key="item.message"> {{ item }} </li>
      </ul>

      <hr>

      <br>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',

  data: function() {
    return {
      trivia: [],
      math: [],
      year: [],
      searchText: '',
      selected: 'trivia',
      options: [
        { text: 'Trivia', value: 'trivia' },
        { text: 'Year', value: 'year' },
        { text: 'Math', value: 'math' }
      ]
    }
  },

  methods: {
    dohvatiPodatke: function(number, type) {
      console.log(number);
      console.log(type);
      this.axios.get('http://numbersapi.com/' + number + '/' + type).then((response) => {

        if (type == 'trivia') {
          if (this.trivia.indexOf(response.data) === -1) {
            this.trivia.push(response.data);
          }
        } else if (type == 'math') {
          if (this.math.indexOf(response.data) === -1) {
            this.math.push(response.data);
          }
        } else if (type == 'year') {
          if (this.year.indexOf(response.data) === -1) {
            this.year.push(response.data);
          }
        }

      })
    },

    // vraća random broj
    randomNumber: function() {
      return Math.floor(Math.random() * 500) + 1;
    }
  },

  // poziva se kada se stranica otvori
  // dohvaća podatke sa api-a
  created: function() {
    for (var i = 0; i < 5; i++) {
      this.dohvatiPodatke(this.randomNumber(), 'trivia');
      this.dohvatiPodatke(this.randomNumber(), 'year');
      this.dohvatiPodatke(this.randomNumber(), 'math');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
ul li {
  list-style-type: none;
}

.bv-no-focus-ring {
  margin-right: 10px;
}

#radio div.form-check {
  justify-content: left;
}

.content {
  margin-top: 50px;
}

.btn.btn-success {
  margin-left: 20px;
  height: 32px;
  align-self: center;
}
</style>
