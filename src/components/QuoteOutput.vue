<template>
  <div class="details-display">
    <div>
      <ul class="details-summary">
        <li class="details-summary-item">
          <div class="details-summary-total data">
            <p>Total amount repayable</p>
            <p class="details-summary-value">
              <span>£ {{ totalAmountRepayable }}</span>
            </p>
          </div>
        </li>
        <li class="details-summary-item">
          <div class="data">
            <p>Monthly Total</p>
            <p class="details-summary-value">
              <span>£ {{ totalMonthly }}</span>
            </p>
          </div>
        </li>
        <li class="details-summary-item">
          <div class="data">
            <p>Loan Amount</p>
            <p class="details-summary-value">
              <span>£ {{ loanAmount }}</span>
            </p>
          </div>
        </li>
        <li class="details-summary-item">
          <div class="data">
            <p>Representative APR</p>
            <p class="details-summary-value">{{ apr }}%</p>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "QuoteOutput",
  props: {
    apr: Number,
    loanAmount: Number,
    totalMonthly: Number,
    totalAmountRepayable: Number,
    loanPeriod: Number,
  },
};
</script>
<style lang="scss" scoped>
.details-summary {
  width: 100%;
  height: auto;
  list-style: none;
  padding: 0;
  .details-summary-item {
    width: 100%;
  }

  li div {
    align-items: center;
  }
}

.details-display {
  width: 100%;
  height: auto;
  border-radius: 10px;
  margin: 30px 0;
}
</style>
