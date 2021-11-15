<template>
  <Modal v-show="isModalVisible" @close="closeModal" />
  <div id="loan-calculator">
    <div
      class="loan-calculator-container"
      :class="{ showCalculator: !showSummaryDisplay }"
    >
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
              pattern="[A-Za-z]"
            />
            <p v-if="nameNoValid" class="inputRequired">*required</p>
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
            <p v-if="surnameNoValid" class="inputRequired">*required</p>
          </div>
        </div>

        <div class="loan-amount">
          <label>Loan Amount</label>
          <p class="increments">(increments of £100)</p>
          <div>
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
          <p class="increments">(increments of 6 months)</p>
          <div class="loan-period-input">
            <input disabled v-model="loanApp.repaymentPeriod" />
            <span>months</span>
          </div>
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

      <div class="quote-output">
        <h2>Quote Output</h2>
        <QuoteOutput
          :apr="loanApp.apr"
          :loan-amount="loanApp.loanAmount"
          :total-monthly="loanApp.monthlyTotal"
          :total-amount-repayable="loanApp.totalAmountRepayable"
          :loan-period="loanApp.repaymentPeriod"
        />
      </div>
    </div>

    <div class="quote-output" v-if="showSummaryDisplay">
      <h2>Quote Summary</h2>
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
        {{ showSummaryDisplay ? "Back to Loan Calculator" : "Show Summary" }}
      </button>
      <button v-if="!showSummaryDisplay" @click="resetCalc()">Reset</button>
    </div>
  </div>
</template>

<script>
import QuoteOutput from "./QuoteOutput";
import QuoteSummary from "./QuoteSummary";
import Modal from "./Modal.vue";
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
      surnameNoValid: null,
      isModalVisible: false,
    };
  },
  components: {
    Slider,
    QuoteOutput,
    QuoteSummary,
    Modal,
  },
  methods: {
    showQuote() {
      if (
        this.showSummaryDisplay === false &&
        this.nameNoValid === false &&
        this.surnameNoValid === false
      ) {
        this.showSummaryDisplay = true;
      } else if (
        this.showSummaryDisplay === false &&
        (this.nameNoValid === null ||
          this.nameNoValid === true ||
          this.surnameNoValid === null ||
          this.surnameNoValid === true)
      ) {
        this.isModalVisible = true;
      } else {
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
    closeModal() {
      this.isModalVisible = false;
    },
  },
};
</script>

<style src="@vueform/slider/themes/default.css"></style>
<style lang="scss">
label {
  margin: 10px 0;
}

button {
  font: inherit;
  cursor: pointer;
  padding: 1rem 2rem;
  border: 1px solid #40005d;
  background-color: #40005d;
  color: white;
  border-radius: 12px;
  margin: 0.1rem auto;
}

#loan-calculator {
  align-items: center;
  max-width: 900px;
  padding: 30px;
  padding-top: 0px;
  margin: 0 auto;
  h1 {
    font-size: 35px;
    font-weight: bold;
  }
}

#customer-options {
  text-align: center;
  border-radius: 12px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
  padding: 1rem;
  padding-bottom: 3rem;
  background-color: rgb(31, 31, 31);
  margin: 2rem auto;
  width: 50rem;
  max-width: 95%;
}

.loan-calculator-container {
  width: 100%;
  display: none;
}

.showCalculator {
  display: block;
}

.customer-details {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 1rem;
  text-align: left;
  label {
    font-weight: bold;
    margin-bottom: 0.5rem;
    display: block;
  }
  input {
    font: inherit;
    padding: 0.5rem;
    border-radius: 6px;
    border: 1px solid #ccc;
    width: 20rem;
    max-width: 100%;
  }
}

.loan-amount {
  width: 100%;
  margin-bottom: 2rem;
  label {
    font-weight: bold;
    margin-bottom: 0.5rem;
    margin-top: 2rem;
    display: block;
  }
  .loan-amount-input {
    padding: 0.5rem;
    width: 100%;
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

.repayment-period {
  display: block;
  width: 100%;
  label {
    font-weight: bold;
    margin-bottom: 0.5rem;
    margin-top: 3rem;
    display: block;
  }
  .loan-period-input {
    padding: 0.5rem;
    width: 100%;
  }
  input {
    width: 30px;
    height: 40px;
    margin-right: 10px;
    display: inline-block;
    text-align: center;
  }
}

.quote-output {
  background-color: #a892ee;
  color: black;
  padding: 1rem;
  margin: 2rem auto;
  width: 50rem;
  max-width: 95%;
  border-radius: 12px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
}

.inputRequired {
  font-size: 0.8rem;
  color: #ec8282;
  margin: 0rem;
}

.slider-red {
  --slider-connect-bg: #ef4444;
  --slider-tooltip-bg: #ef4444;
  --slider-handle-ring-color: #ef444430;
}

.increments {
  font-size: 0.7rem;
  margin: 0rem;
}

.footer {
  width: 100%;
  display: flex;
  justify-content: space-around;
}
</style>
