<template>
  <div class="quote-summary">
    <div class="summary-details"></div>
  </div>
</template>
<script>
export default {
  props: {
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
.quote-summary {
  display: block;
  margin-bottom: 20px;
}

.summary-details {
  max-width: 800px;
  text-align: center;
  box-sizing: border-box;

  p {
    font-size: 18px;
  }
}
</style>
