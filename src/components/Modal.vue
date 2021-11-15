<template>
  <div class="modal">
    <header class="header">
      <h2>First name or last name incorrect</h2>
    </header>
    <div class="content">
      <p>Please, introduce your First Name and Last Name.</p>
      <p>Remember that they require a minimum of three characters each.</p>
    </div>
    <footer class="actions">
      <button type="button" class="btn-green" @click="close">
        Ok, I will do it!
      </button>
    </footer>
  </div>
</template>

<script>
export default {
  name: "Modal",
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
  position: fixed;
  top: 30vh;
  left: 10%;
  width: 80%;
  z-index: 100;
  overflow: hidden;
  background: rgba(0, 0, 0, 1);
}

.header {
  background: #4f005f;
  padding: 1rem;
}

.header h2 {
  margin: 0;
  color: white;
}

.content {
  padding: 1rem;
}

.actions {
  padding: 1rem;
  display: flex;
  justify-content: flex-end;
}

@media (min-width: 768px) {
  .modal {
    left: calc(50% - 20rem);
    width: 40rem;
  }
}
</style>
