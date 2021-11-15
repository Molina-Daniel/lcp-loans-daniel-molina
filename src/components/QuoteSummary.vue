<template>
  <div>
    <div class="summary-details">
      <div class="client-details">
        <p>{{ firstName }} {{ lastName }}</p>
      </div>

      <div class="client-loan-details">
        <div class="loan-data">
          <p>Total amount repayable</p>
          <p class="details-summary-value">
            <span>£ {{ totalAmountRepayable }}</span>
          </p>
        </div>
        <div class="loan-data">
          <p>Monthly Total</p>
          <p class="details-summary-value">
            <span>£ {{ totalMonthly }}</span>
          </p>
        </div>
        <div class="loan-data">
          <p>Loan Amount</p>
          <p class="details-summary-value">
            <span>£ {{ loanAmount }}</span>
          </p>
        </div>
        <div class="loan-data">
          <p>Representative APR</p>
          <p class="details-summary-value">{{ apr }}%</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    apr: Number,
    firstName: String,
    lastName: String,
    loanAmount: Number,
    totalAmountRepayable: Number,
    totalMonthly: Number,
    loanPeriod: Number,
  },
};
</script>
<style lang="scss" scoped>
.client-details {
  display: flex;
  color: white;
  border-radius: 12px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
  background-color: rgb(31, 31, 31);
  margin-bottom: 1rem;
  justify-content: space-around;
  p {
    margin: 1rem;
    font-size: 2rem;
  }
}

.client-loan-details {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1rem;
  align-items: center;
  padding-inline-start: 0px;
}

.loan-data {
  font-size: 1rem;
  flex: 1;
  margin: 1rem 1rem;
  color: white;
  border-radius: 12px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
  background-color: rgb(31, 31, 31);
  p {
    margin: 0.5rem;
  }
}
</style>
