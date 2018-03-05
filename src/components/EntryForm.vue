<template lang="html">
  <form  action="index.html" method="post" class="row center-align">
    <div class="col s10 m6 l5 form-wrapper">
    <div class="row">
      <div class="input-field col s12">
        <input id="email" type="text" class="validate">
        <label for="email">Email</label>
      </div>
      <div class="input-field col s6">
        <select>
          <option value="" disabled selected>Month</option>
          <option v-for="(month, index) in months" :value="months.indexOf(month)" :key="index">{{month}}</option>
        </select>
        <label>Month of birth</label>
      </div>
      <p class="col s1 month-year-divider">/</p>
      <div class="input-field col s5 dob-year">
        <select>
          <option value="" disabled selected>Year</option>
          <option v-for="(year, index) in years(1900, 2010)" :value="year" :key='index' >{{year}}</option>
        </select>
        <label>Year of birth</label>
      </div>
      <div class="input-field col s12">
        <input id="zip" type="number" class="validate">
        <label for="zip">Zip Code</label>
      </div>
      <div class="input-field col s12">
        <input id="gender" type="text" class="validate">
        <label for="gender">Gender</label>
      </div>
      <p class="col s12 checkbox-wrapper">
        <label>
         <input type="checkbox" id="tc_agreed" />
         <span for="tc_agreed">I accept <a href="https://www.google.com/" target="_blank">Terms of Service</a></span>
       </label>
     </p>

     <h6 class="col s12 newsletter-header">{{newsletter_header}}</h6>
     <p class="col s12 checkbox-wrapper">
       <label>
        <input type="checkbox" id="newsletter" />
        <span for="newsletter" class="newsletter-description" v-html="newsletter_description"></span>
      </label>
    </p>
      <div class="input-field col s12 submit-wrapper">
        <input id="submit" type="submit" class="btn validate" value="submit">
      </div>

    </div>
  </div>
  </form>
</template>

<script>
import M from 'materialize-css'

export default {
  name: 'EntryForm',
  props: ['model'],
  data () {
    return {
      months: [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ],
      years (from, to) {
        let res = []
        for (let i = to; i > from; i -= 1) {
          res.push(i)
        }
        return res
      },
      newsletter_header: '',
      newsletter_description: ''
    }
  },
  mounted () {
    this.$el.querySelectorAll('select').forEach(function (elem) {
      M.FormSelect.init(elem)
    })
    this.newsletter_header = this.model.newsletter_header
    this.newsletter_description = this.model.newsletter_description
  }
}
</script>

<style lang="css" >
.form-wrapper{
  display: inline-block;
  float: none!important;
}
.input{
  font-family: 'Mukta Malar', sans-serif;
}

.month-year-divider{
  vertical-align: bottom;
    line-height: 4rem;
}
.dob-year .dropdown-content li>span{
  font-size: 1.3rem;
}
p.checkbox-wrapper{
  float:left;
  margin-left: 2rem;
  margin-bottom: 0;
  text-align: left;
}
.col.submit-wrapper{
  margin-top: 2rem;
}

.newsletter-header{
  font-size: 1rem;
  margin-top: 2rem;
  margin-bottom: 0;
  text-align: left;
}

.newsletter-description{
  max-width: 100%;
  text-align: left;
}
</style>
