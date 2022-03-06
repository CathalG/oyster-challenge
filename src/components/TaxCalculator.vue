<template>
  <div class="calculator-container">
    <h2>Enter details to get started:</h2>
    <div id="calculator-inputs">
        <input id="gross-salary" name="gross-salary" v-model="gross_salary" @keyup="calculateTax()" placeholder="Enter Gross Salary...">
        <select id="country" v-model="selected_country" @change="calculateTax()">
          <option value="null">Choose A Country</option>
          <option v-for="country in tax_rates.countries" :key="country.id" :value="country.id" >{{country.name}}</option>
        </select>
    </div>

    <div class="results-container" :class="{active: tax_obligations.employer }">
      <div>
        <div class="result-header">
          <h3>Employer:</h3>
        </div>
       <div class="results-details">
         <p v-for="obligation in tax_obligations.employer" :key="obligation.name">{{obligation.name}} : {{obligation.amount}}</p>
        </div>
        <div class="result-total">
          <p v-if="tax_obligations.employer">Total {{employerTotal}}</p>
        </div>
      </div>
      <div>
        <div class="result-header">
          <h3>Employee:</h3>
        </div>
        <div class="results-details">
           <p v-for="obligation in tax_obligations.employee" :key="obligation.name">{{obligation.name}} : {{obligation.amount}}</p>
        </div>
        <div class="result-total">
          <p v-if="tax_obligations.employee">Total: {{employeeTotal}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import rates from '.././tax_rates.json'

export default {
  /* eslint-disable no-unused-vars */
  name: 'TaxCalculator',
  data () {
    return {
        gross_salary:null,
        selected_country:null,
        tax_rates: rates,
        tax_obligations:{},
      }
    },
    methods:{
      calculateTax(){
          if(this.gross_salary!=null && this.selected_country!=null){
            this.tax_obligations = {};
            //loop through tax categories
            for (const [name, category] of Object.entries(this.tax_rates.countries[this.selected_country].taxes)) {
                let obligations = [];
                //then loop through each individual tax
                category.forEach(tax => {
                  let amount = 0;
                    //if tax is simple percentage calculate and add to tax obligations
                    if(tax.type == "percentage"){
                      amount = (this.gross_salary/100) * tax.rate
                    }
                    //if it's progressive then calculate seperately
                    else if(tax.type == "progressive"){            
                      amount = this.progessiveTaxObligations(tax.rates)
                    }
                    //save the tax name and amount owed
                    obligations.push({
                      "name":tax.name,
                      "amount":amount.toFixed(2)
                    });
                })
                //Add the category to the tax_obligations object
                Object.defineProperty(this.tax_obligations, name, {
                  value: obligations,
                  writable: true,
                  enumerable: true
                });
            }
          }
      },
      progessiveTaxObligations(rates){
        let taxable_income = this.gross_salary;
        let total = 0; 
        //loop through each tax band
        rates.forEach(band => {
            if(taxable_income > 0){
              total += taxable_income>band.limit && band.limit ? (band.limit/100)*band.rate : (taxable_income/100)*band.rate;
              band.limit?taxable_income -= band.limit:taxable_income = 0;
            }
        })
        return total;
      } 
    },
    computed: {
      employeeTotal() {
        let total = 0;
        this.tax_obligations.employee.forEach(tax => {
          total += parseInt(tax.amount);
        });
        return total.toFixed(2);
      },
      employerTotal() {
        let total = 0;
        this.tax_obligations.employer.forEach(tax => {
          total += parseInt(tax.amount);
        });
        return total.toFixed(2);
      }
    }
  }
</script>

<style scoped>

  h2{
    text-align: left;
  }

 div#calculator-inputs input,div#calculator-inputs select {
    outline: 0;
    padding: 10px;
    border:1px solid #adb7c6;
    font-size:16px;
    background: #FFF;
    min-height: 20px;
    min-width: 0 !important;
    flex: 1 1 50% !important;
}

.results-container > div {
    flex: 1;
    justify-content: flex-start;
    align-items: flex-start;
    justify-items: flex-start;
    text-align: left;
    display: flex;
    flex-direction: column;
    padding: 0 15px;
}

.results-container h3 {
    margin: 5px 0px;
}

div#calculator-inputs {
    display: flex;
    justify-content: center;
}

div#calculator-inputs select {
    border-left: 0 !important;
    cursor: pointer;
}

.calculator-container {
    display: flex;
    flex-direction: column;
}

.results-container{
    opacity: 0;
    pointer-events: none;
    height: 0;
    overflow: hidden;
    transition: all 0.2s ease-in-out;
    background: #FFF;
    
    border: 1px solid #bfc7d2;
    border-radius: 5px;
    margin-top: 15px;
    display: flex;
   flex-direction: row;
}

.results-container.active{
    opacity: 1;
    height: 230px;
    pointer-events: all;
    padding: 25px 20px;
}

.result-total {
    margin-top: auto;
}
</style>
