<template>
  <header>
    <div class="logo">Campaigns</div>

    <button @click="toggleSidebar()">Filters</button>
  </header>
</template>

<script>
export default {
  name: 'PageHeader',
  props: {
    toggleSidebar: { type: Function, required: true }
  }
}
</script>

<style lang="scss" scoped>
  header {
    background-color: #fff;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.2);
    padding: 1rem;
    width: 100%;
    height: 65px;

    position: fixed;
    top: 0;
    left: 0;
    z-index: 50;

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    font-size: 1.7rem;
    color: #5d5d5d;
  }
</style>
