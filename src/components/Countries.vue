<template>
<div class="container row">
     <br><br>
      <h1 class="title">{{ title }}</h1>

        <div class="column country-info">
            <form>
                    <div class="form-controls">
                        <label for="countries" class="select-label">Select Country</label>
                        <select name="countries" id="countries" v-model="selectedCountry" @change="onCountrySelect($event)" class="count-select">
                            <option :value="country.name" v-for="country in countries" :key="country.id" class="count-option">{{ country.name }}</option>
                        </select>
                    </div>
                    <div class="form-controls">
                        <label for="state" class="select-label">Select State</label>
                        <select name="states" id="states" v-model="selectedState" @change="onStateSelect($event)" class="count-select">
                            <option :value="state.name" v-for="state in states" :key="state.id" class="count-option">{{ state.name}}</option>
                        </select>
                        <div v-if="stateError" :class="{empty: stateError}">
                            <p>This Country Has No States!</p>
                        </div>
                    </div>
                    <div class="form-controls">
                        <label for="cities" class="select-label">Select City</label>
                        <select name="cities" id="cities" v-model="selectedCity" class="count-select">
                            <option :value="city.name" v-for="city in cities" :key="city.id" class="count-option">{{ city.name}}</option>
                        </select>
                        <div v-if="cityError" :class="{empty: cityError}">
                            <p>This State Has No Cities!</p>
                        </div>
                    </div>      
            </form>
        </div>

        <!-- Selected -->
        <div class="column-2">
            <div class="selected-options">
            <p v-if="selectedCountry"><span class="s-o">Country:</span> {{ selectedCountry }}</p>
            <br>

            <p v-if="selectedState"><span class="s-o">State:</span> {{ selectedState }}</p>
            <br>

            <p v-if="selectedCity"><span class="s-o">City:</span> {{ selectedCity }}</p>
            </div>
        </div>
</div>
</template>

<script>
export default {
  name: 'countries',
  data() {
     return {
            title: 'WeJapa Countries',
            selectedCountry: "",
            selectedState: "",
            selectedCity: "",
            stateError: false,
            cityError: false,
            countries: [],
            states: [],
            cities: []
        }
    },
    created() {
        this.$http.get('https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json').then(
            response=> {
                return response.json()
            }
        ).then(data=> {
            this.countries = data.slice(0, 251);
        })    
    },
    methods: {
        onCountrySelect(event) {
            const state = event.target.value
            const index = this.countries.findIndex((x=>x.name === state))
            const countryState = this.countries[index]['states']
            if (countryState.length === 0) {
                this.stateError = !this.stateError 
                this.states = null   
                this.cities = ""
            } else {
                this.states = countryState
                this.stateError = false
            }
        },
        onStateSelect(event) {
            const state = event.target.value
            const index = this.states.findIndex((x=>x.name === state))
            const city = this.states[index]['cities']
            if (city.length === 0) {
                this.cityError = !this.cityError
            } else {
                this.cities = city
                this.cityError = false
            }
        }
    }
}
</script>


<style scoped>
.title{
    margin-top: 1px;
   text-align: center;
   font-size: 35px;
}
.select-label{
    margin-bottom: 30px;
    font-family: 'Poppins', sans-serif;
    font-size: 3vh;
}
.country-info{
    padding: 50px;
}
.form-controls{
    margin: 10px;
}
.count-select{
    background: none repeat scroll 0 0 #FFFFFF;
    border: 2px solid rgb(27, 25, 25);
    border-radius: 10px;
    height: 40px;
    margin: 20px 0;
    font-size: 13px;
    display: block;
    padding: 10px;
    width: 290px;
    margin-bottom: 4px;
    outline: none;
}
.count-option{
    display: block;
    margin-left: 20px;
    font-size: 14px;
}
.column-1{
    float: left;
    width: 20%;
    padding-left: 20% !important;
    height: 75vh;
    margin: auto
}
.column-2{
    padding-left: 5% !important;
}
.row:after {
  content: "";
  display: table;
  clear: both;
}
.empty{
    border: 1px solid;
    background-color: rgb(255, 255, 255);
    color: black;
    width: 15em;
    height: 100px;
    padding: 20px;
    border-radius: 6px;
    margin-top: 10px;
}
.selected-options{
    margin-top: 10px;
}
.selected-options p {
    margin-bottom: 1px;
}
@media screen and (max-width: 1024px) {
  .column-1 {
    float: left;
    width: 50%;
    padding-left: 12% !important;
    height: 100vh;
}
.column-2 {
    float: left;
    width: 20%;
    padding-left: 10% !important;
    height: 100vh;
}
}
@media screen and (max-width: 768px) {
  .column-2 {
    float: left;
    width: 20%;
    padding-left: 8% !important;
    height: 100vh;
}
.column-2 {
    float: left;
    width: 20%;
    padding-left: 10% !important;
    height: 100vh;
}
}
@media screen and (max-width: 575px) {
  .column-1 {
    float: left;
    width: 20%;
    padding-left: 8% !important;
    height: 100vh;
}
.column-2 {
    position: absolute;
    margin-top: 120% !important;
    margin-bottom: 2em;
    padding-left: 8% !important;
    left: 50%;
    transform: translateX(-50%) !important;
    width: 50% !important;
    height: auto;
}
}
@media screen and (max-width: 414px) {
  .column-1, .column-2 {
    padding-left: 8% !important;
}
} 
</style>