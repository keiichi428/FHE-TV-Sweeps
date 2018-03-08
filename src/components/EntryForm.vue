<template lang="html">
  <form id="entry" action="https://forms.foxfilm.com/sqs/signup_handler.php" method="get" class="row center-align" >

    <input type="hidden" name="REG_SOURCE" v-if="model" :value="model.campaign_id" class="testregsource">
    <input type="hidden" name="ri_url" value="https://foxus.rsys2.net/pub/rf" />
    <input type="hidden" name="_ri_"  value="X0Gzc2X%3DWQpglLjHJlTQGrGIIwUiGBcnIh2lDhzdrcv9L6nVwjpnpgHlpgneHmgJoXX0Gzc2X%3DWQpglLjHJlTQGsrzaNizbJPtIaBRuRK0riwSDtDO" class="idtest">

    <div class="col s12 m8 form-wrapper">
        <h4 class="row text-official-entry-form">
          Official Entry Form
      </h4>
    <div class="row">
      <div class="input-field-custom col s12">
        <div class="input-wrapper">
          <input id="email" type="email" class="validate input-text" name="email_address_">
        </div>
        <label for="email">Email*</label>
      </div>
      <div class="input-field col s6 dob-month">
        <select name="dob_month">
          <option value="" disabled selected> </option>
          <option v-for="(month, index) in months" :value="months.indexOf(month) + 1" :key="index">{{month}}</option>
        </select>
        <label>Month of birth*</label>
      </div>
      <div class="input-field col s6 dob-year">
        <select name="dob_year">
          <option value="" disabled selected> </option>
          <option v-for="(year, index) in years(1919, 2010)" :value="year" :key='index' >{{year}}</option>
        </select>
        <label>Year of birth*</label>
      </div>
      <div class="input-field-custom col s6">
        <div class="input-wrapper">
          <input id="zip" type="number" name="zip" class="validate input-text">
        </div>
        <label for="zip">Zip Code*</label>
      </div>
      <div class="input-field col s6 gender">
        <select name="gender">
          <option value="" disabled selected> </option>
          <option value="male" >Male</option>
          <option value="female" >Female</option>
        </select>
        <label>Gender*</label>
      </div>

      <hr class="col s12" />

      <p class="input-field-custom col s12 checkbox-wrapper">
        <label>
         <input type="checkbox" id="tc_agreed" />
         <span for="tc_agreed">By clicking submit below, you consent to the Fox
   <a href="http://www.foxprivacy.com/us/privacy.html" target="_blank">Privacy Policy</a>
   and
   <a href="http://www.foxprivacy.com/us/terms.html" target="_blank">Terms of Use</a>.</span>
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
import SmoothScroll from 'smoothscroll'
const ERR_EMAIL_EMPTY = "Email address is required.",
ERR_DOB_MONTH_EMPTY = "Month of birth is required.",
ERR_ZIP_EMPTY = "Zip code is required.",
ERR_GENDER_EMPTY = "Gender is required."
let dobSelectors, theForm

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
  methods: {
    closeDobSelectors () {
      if (!dobSelectors) {
        console.error('dobSelectors is not defined')
        return
      }
      dobSelectors.forEach((elem) => {
        // console.log(elem.M_FormSelect);
        elem.M_FormSelect.dropdown.close()
      })
    },

    checkForScrollHash () {
      this.$nextTick(function () {
        if (location.hash.indexOf('entry') !== -1) {
          let entry = document.querySelector('#entry')
          if (entry) { SmoothScroll(entry) }
        }
      })
    },

    initDobSelector () {
      dobSelectors = this.$el.querySelectorAll('select')
      dobSelectors.forEach(function (elem) {
        elem.m =
        M.FormSelect.init(elem, {
          dropdownOptions: {
            closeOnClick: false
            // ,constrainWidth: false
          }})
      })
      const vm = this
      this.$el.querySelectorAll('.dropdown-content li>span').forEach(function (elem) {
        elem.addEventListener('click', function (e) {
          vm.closeDobSelectors()
        })
      })
    },

    initNewsletterTexts () {
      this.newsletter_header = this.model.newsletter_header
      this.newsletter_description = this.model.newsletter_description
    },

    initValidation (){
      theForm = this.$el
      theForm.addEventListener("submit", this.onFormSubmit)
    },

    onFormSubmit(e){
      console.log("onFormSubmit")
      e.preventDefault()
      this.$el.querySelectorAll('.error').forEach(function(elem){
        elem.classList.remove('error')
      });
      console.log(this.$el.querySelectorAll('.error'))
      let errors = []
      const
      i_email = this.$el.querySelector('input[name=email_address_]'),
      s_dob_month = this.$el.querySelector('select[name=dob_month]'),
      i_dob_month = this.$el.querySelector('.dob-month input'),
      s_dob_year = this.$el.querySelector('select[name=dob_year]'),
      i_dob_year = this.$el.querySelector('.dob-year input'),
      i_zip = this.$el.querySelector('input[name=zip]'),
      s_gender = this.$el.querySelector('select[name=gender]'),
      i_gender = this.$el.querySelector('.gender input')

      // email - empty?
      if(!i_email.value || i_email.value.length < 0){
        errors.push(ERR_EMAIL_EMPTY)
        i_email.classList.add('error')
      }

      // month of birth - unselected?
      if(!s_dob_month.value || s_dob_month.value === '' ){
        errors.push(ERR_DOB_MONTH_EMPTY)
        i_dob_month.classList.add('error')
      }

      // year of birth - unselected?
      if(!s_dob_year.value || s_dob_year.value === '' ){
        errors.push(ERR_DOB_MONTH_EMPTY)
        i_dob_year.classList.add('error')
      }

      // zip - empty?
      if(!i_zip.value || i_zip.value.length < 0){
        errors.push(ERR_ZIP_EMPTY)
        i_zip.classList.add('error')
      }

      // gender - unselected?
      if(!s_gender.value || s_gender.value === '' ){
        errors.push(ERR_GENDER_EMPTY)
        i_gender.classList.add('error')
      }
    }
  },
  mounted () {
    this.initDobSelector()
    this.initNewsletterTexts()
    this.checkForScrollHash()
    this.initValidation()
  }
}
</script>

<style lang="css"  >
.row .col.form-wrapper{
  display: inline-block;
  float: none!important;
  padding: 1rem ;
  margin-top: 2rem;
  max-width: 450px;
  background: #fff;
  border-radius: 10px;
}
@media (min-width:601px) {
  .row .col.form-wrapper{
    padding: 1rem 3rem;
  }
}

h4.text-official-entry-form{
  font-size: 2rem;
}

label{
  float: left;
}
label, .input{
  font-family: Helvetica, arial, sans-serif;
}

.input-field,
.input-field-custom {
  position: relative;
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.select-wrapper+label,
.input-field+label,
.input-field-custom label{
  position: absolute;
  top: -20px;
  font-size: .8rem;
}
.input-field.col label,
.input-field-custom.col label {
    left: .75rem;
    text-align: left;
}
.input-field-custom label{
  transform: translateY(12px);
}
.input-field>label,
.input-field-custom>label{
  color: #000;
  font-weight: 600;
  letter-spacing: 0.01em;
}

input[type=text].input-text:focus:not([readonly]),
input[type=number].input-text:focus:not([readonly]){
    box-shadow: none;
    border-bottom: 1px solid #d7d7d7;
}



input[type=text].input-text,
input[type=email].input-text,
input[type=number].input-text{
  border: 1px solid #d7d7d7;
  height: 22px;
  background: #f2f2f2;
  margin-top: 1rem;
}


.month-year-divider{
  vertical-align: bottom;
  line-height: 4rem;
}
.dob-year .dropdown-content.select-dropdown{
  max-height: 300px;
}
.dob-year .dropdown-content li>span{
  font-size: 1.0rem;
}

p.checkbox-wrapper{
  float:left;
  margin-left: 2rem;
  margin-bottom: 0;
  text-align: left;
}

.checkbox-wrapper span{
  color: #bfbfbf;
  font-family: Helvetica, arial, sans-serif;
}
.checkbox-wrapper span a{
  color: #808080;
}

.input-wrapper{
  position: relative;
  box-sizing: border-box;
}
.select-wrapper{
  background: #f2f2f2;
}
.select-wrapper input.select-dropdown{
  border: 1px solid #d7d7d7;
  height: 22px;
  border-radius: 3px;
  padding-left: .5rem;
  width: calc(100% - .5rem);
  margin-top: 1rem;
}
.select-wrapper input.select-dropdown:focus{
  border: 1px solid #d7d7d7;
}
.select-wrapper .caret{
  fill: #fff;
  background: #808080;
  border-radius: 3px;
  margin: 0;
  right: -1px;
}
.dropdown-content li{
  line-height: 1.25em;
  min-height: 1em;
  font-family: arial, sans-serif;
}
.dropdown-content li>a, .dropdown-content li>span{
  color: #000;
  padding: 4px 10px;
}
.col.submit-wrapper{
  margin-top: 2rem;
}

.btn#submit{
  font-family:'Oswald', sans-serif;
  font-size: 1.24rem;
  line-height: 1.0em;
  height: 2rem;
  letter-spacing: 0.05em;
  background:#d57c07;
  padding: 0 15px;
  border-radius: 5px;
  transition: all .3s;
  box-shadow: inset 1px 1px 1px rgba(255,255,255,.7), inset -1px -1px 1px rgba(0,0,0,.7);
}

.btn#submit:hover{
  box-shadow: inset 1px 1px 1px rgba(255,255,255,.4), inset -1px -1px 1px rgba(0,0,0,.4);
}

[type="checkbox"]+span:not(.lever){
  font-size: .75rem;
}
[type="checkbox"]+span:not(.lever):before, [type="checkbox"]:not(.filled-in)+span:not(.lever):after{
  background: #f2f2f2;
  border-color: #bfbfbf;
}
[type="checkbox"]:checked+span:not(.lever):before{
  background: transparent;
  border-right: 2px solid #777;
  border-bottom: 2px solid #777;
}
.newsletter-header{
  font-size: 1.25rem;
  font-weight: 900;
  font-family:'Oswald', sans-serif;
  margin-top: 2rem;
  margin-bottom: 0;
  text-align: center;
  color: #d57c07;
}

[type="checkbox"]#tc_agreed+span:not(.lever),
[type="checkbox"]+span:not(.lever).newsletter-description{
  font-weight: 100;
  max-width: 100%;
  text-align: left;
  line-height: 1.5em;
}
input[type=text].input-text.error,
input[type=email].input-text.error,
input[type=number].input-text.error,
.select-wrapper input.select-dropdown.error{
  border: 1px solid pink;
}
</style>
