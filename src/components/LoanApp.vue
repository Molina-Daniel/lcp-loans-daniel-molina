<template>
  <div id="loan-calculator">
    <div
      class="loan-calculator-container"
      :class="{ showCalculator: !showSummaryDisplay }"
    >
      <h2>LCP LOANS</h2>

      <form id="customer-options">
        <div class="customer-details-wrapper">
          <div class="customer-details">
            <label for="first Name">First Name:</label>
            <input
              id="firstname"
              type="text"
              minlength="3"
              @input="onNameInputChange"
              v-model.trim="loanApp.firstName"
              :class="{ invalidInput: nameNoValid }"
              ref="firstname"
              placeholder="Minimum 3 characteres"
              autofocus
            />
            <!-- <p v-if="onNameBlur" class="invalidInput">
              Please, your name has to be at least 3 characteres
            </p> -->
          </div>

          <div class="customer-details">
            <label>Last name:</label>
            <input
              id="lastname"
              type="text"
              minlength="3"
              @input="onSurnameInputChange"
              v-model.trim="loanApp.lastName"
              :class="{ invalidInput: surnameNoValid }"
              ref="lastname"
              placeholder="Minimum 3 characteres"
            />
            <!-- <p v-if="onSurnameBlur" class="invalidInput">
              Please, your name has to be at least 3 characteres
            </p> -->
          </div>
        </div>

        <div>
          <label>Loan Amount</label>
          <div class="loan-amount">
            <div class="loan-amount-input">
              <span>£</span>
              <input disabled v-model="loanApp.loanAmount" />
            </div>
            <div class="slide-container">
              <Slider
                class="slider-red"
                v-model="loanApp.loanAmount"
                :min="100"
                :max="2500"
                :step="100"
                tooltipPosition="bottom"
                @change="calcTotalAmtRepay"
              ></Slider>
            </div>
          </div>
        </div>

        <div class="repayment-period">
          <label>Loan Period</label>
          <Slider
            v-model="loanApp.repaymentPeriod"
            @change="calcMonthlyTotal"
            :min="6"
            :max="36"
            :step="6"
            tooltipPosition="bottom"
          ></Slider>
        </div>
      </form>

      <QuoteOutput
        :apr="loanApp.apr"
        :loan-amount="loanApp.loanAmount"
        :total-monthly="loanApp.monthlyTotal"
        :total-amount-repayable="loanApp.totalAmountRepayable"
        :loan-period="loanApp.repaymentPeriod"
      />
    </div>

    <div v-if="showSummaryDisplay">
      <QuoteSummary
        :first-name="loanApp.firstName"
        :last-name="loanApp.lastName"
        :apr="loanApp.apr"
        :loan-amount="loanApp.loanAmount"
        :total-monthly="loanApp.monthlyTotal"
        :total-amount-repayable="loanApp.totalAmountRepayable"
        :loan-period="loanApp.repaymentPeriod"
        :show-summary-dispay="showSummaryDisplay"
      />
    </div>

    <div class="footer">
      <button @click="showQuote()">
        {{ showSummaryDisplay ? "Loan Calculater" : "Show Summary" }}
      </button>
      <button v-if="!showSummaryDisplay" @click="resetCalc()">Reset</button>
    </div>
  </div>
</template>

<script>
import QuoteOutput from "./QuoteOutput";
import QuoteSummary from "./QuoteSummary";
import Slider from "@vueform/slider";

export default {
  data() {
    return {
      loanApp: {
        firstName: "",
        lastName: "",
        loanAmount: 100,
        totalAmountRepayable: 125,
        monthlyTotal: 20.83,
        repaymentPeriod: 6,
        apr: 125,
      },
      showSummaryDisplay: false,
      nameNoValid: null,
      // onNameBlur: null,
      surnameNoValid: null,
      // onSurnameBlur: null,
      // formatter: (v) => `£${("" + v).replace(/\B(?=(\d{3})+(?!\d))/g, ",")}`,
    };
  },
  components: {
    Slider,
    QuoteOutput,
    QuoteSummary,
  },
  methods: {
    showQuote() {
      if (
        this.showSummaryDisplay === false &&
        this.nameNoValid === false &&
        this.surnameNoValid === false
      ) {
        this.showSummaryDisplay = true;
      } else {
        this.nameNoValid = true;
        this.surnameNoValid = true;
        this.showSummaryDisplay = false;
      }
    },
    resetCalc() {
      this.loanApp.firstName = "";
      this.loanApp.lastName = "";
      this.loanApp.loanAmount = 100;
      this.loanApp.totalAmountRepayable = 125;
      this.loanApp.monthlyTotal = 20.83;
      this.loanApp.repaymentPeriod = 6;
    },
    calcTotalAmtRepay(amount) {
      this.loanApp.totalAmountRepayable = amount * 1.25;
      this.calcMonthlyTotal();
    },
    calcMonthlyTotal() {
      const monthlyTotal =
        this.loanApp.totalAmountRepayable / this.loanApp.repaymentPeriod;
      this.loanApp.monthlyTotal =
        Math.round((monthlyTotal + Number.EPSILON) * 100) / 100;
    },
    // onNameInputBlur() {
    //   if (this.loanApp.firstName.trim().length < 3) {
    //     this.onNameBlur = true;
    //   } else {
    //     this.onNameBlur = false;
    //   }
    // },
    onNameInputChange() {
      if (this.loanApp.firstName.trim().length < 3) {
        this.nameNoValid = true;
      } else {
        this.nameNoValid = false;
      }
    },
    onSurnameInputChange() {
      if (this.loanApp.lastName.trim().length < 3) {
        this.surnameNoValid = true;
      } else {
        this.surnameNoValid = false;
      }
    },
  },
};
</script>

<style src="@vueform/slider/themes/default.css"></style>
<style lang="scss" scoped>
.slider-red {
  --slider-connect-bg: #ef4444;
  --slider-tooltip-bg: #ef4444;
  --slider-handle-ring-color: #ef444430;
}
label {
  display: inline-block;
  margin: 10px 0;
}
#loan-calculator {
  align-items: center;
  max-width: 900px;
  padding: 30px;
  margin: 0 auto;
  h2 {
    font-size: 35px;
    font-weight: bold;
  }
}
.loan-calculator-container {
  width: 100%;
  display: none;
}
.showCalculator {
  display: block;
}

#customer-options {
  height: auto;
}
.customer-details {
  width: 100%;
  height: 50px;
  margin-bottom: 65px;
  box-sizing: border-box;
  label {
    margin-bottom: 5px;
  }
  input {
    width: 100%;
    height: 50px;
  }
}

.loan-amount {
  width: 100%;
  margin-bottom: 30px;
  .loan-amount-input {
    display: flex;
    height: 50px;
    align-items: center;
    box-sizing: border-box;
  }
  input {
    width: 50px;
    height: 40px;
    margin-right: 10px;
    display: inline-block;
    margin-left: 5px;
  }
}
.invalidInput {
  border-color: red;
  background: #fbdada;
}
.slide-container {
  width: 75%;
}

.repayment-period {
  display: block;
  width: 100%;
  .vue-slider {
    margin-bottom: 10px;
  }
}

.footer {
  width: 100%;
  display: flex;
  justify-content: center;
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>
