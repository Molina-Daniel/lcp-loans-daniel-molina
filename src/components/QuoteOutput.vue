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
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 1rem;
  align-items: center;
  padding-inline-start: 0px;
}

.details-summary-item {
  font-size: 1rem;
  flex: 1;
  margin: 0 1rem;
}
</style>
