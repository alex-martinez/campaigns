<template>
  <aside :class="[sidebarVisible ? 'sidebar--visible' : '']" class="sidebar">
    <header class="sidebar-header">
      <div class="sidebar-header__title">Filters</div>
      <button @click="resetFilters()">Reset Filters</button>

      <div @click="toggleSidebar()" class="sidebar-header__close">&times;</div>
    </header>

    <section class="sidebar-filter">
      <label for="sort-by">Sort By</label>
      <div class="select">
        <select name="Sort By" id="sort-by" v-model="selectedSort" @change="sortBy">
          <option value="Name">Name</option>
          <option value="Created">Created</option>
          <option value="Updated">Updated</option>
          <option value="Enabled">Enabled</option>
        </select>
      </div>
    </section>

    <section class="sidebar-filter">
      <label for="order">Order</label>
      <div class="select" >
        <select name="Order" id="order" v-model="selectedOrder" @change="reorder">
          <option value="Ascending">Ascending</option>
          <option value="Descending">Descending</option>
        </select>
      </div>
    </section>
  </aside>
</template>

<script>
  export default {
    name: 'sidebar',
    props: {
      sidebarVisible: { type: Boolean, required: true },
      toggleSidebar: { type: Function, required: true },
      orderCampaignsAscending: { type: Function, required: true },
      orderCampaignsDescending: { type: Function, required: true },
      sortCampaignsByName: { type: Function, required: true },
      sortCampaignsByCreated: { type: Function, required: true }
    },
    data () {
      return {
        selectedSort: 'Created',
        selectedOrder: 'Descending'
      }
    },
    methods: {
      sortBy () {
        switch (this.selectedSort) {
          case 'Name':
            this.sortCampaignsByName()
            break
          case 'Created':
            this.sortCampaignsByCreated()
            break
        }
      },
      reorder () {
        switch (this.selectedOrder) {
          case 'Ascending':
            this.orderCampaignsAscending()
            break
          case 'Descending':
            this.orderCampaignsDescending()
            break
        }
      },
      resetFilters () {
        if (this.selectedOrder !== 'Descending') {
          this.selectedOrder = 'Descending'
          this.orderCampaignsDescending()
        }

        if (this.selectedSort !== 'Created') {
          this.selectedSort = 'Created'
          this.sortCampaignsByCreated()
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  label {
    display: block;
    font-size: .8rem;
    font-weight: bold;
    color: #5e6c75;
  }

  select {
    color: #fff;
    font-size: 0.9rem;
    display: block;
    padding: .5rem 2rem .5rem .5rem;
    width: 100%;

    appearance: none;
    background-color: transparent;
    border-radius: 0;
    border: none;
    border-bottom: 1px solid #5e6c75;

    outline: none;
    cursor: pointer;
  }

  .select {
    position: relative;

    &:after {
      content: "";
      display: block;
      width: 7px;
      height: 7px;
      position: absolute;
      right: 1rem;
      top: 50%;
      transform: translateY(-50%) rotate(-45deg);
      pointer-events: none;
      border-bottom: 1px solid #5e6c75;
      border-left: 1px solid #5e6c75;
    }
  }

  .sidebar {
    width: 350px;
    height: 100%;

    background-color: #35464e;
    color: #fff;

    position: fixed;
    z-index: 100;
    right: 0;
    top: 0;

    transform: translate3d(100%, 0, 0);
    transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);

    &--visible {
      transform: translate3d(0, 0, 0);
    }

    // Header
    // ---------------
    &-header {
      text-align: center;
      font-weight: normal;
      padding: 1rem;
      position: relative;

      &__title {
        font-size: 1.2rem;
        font-weight: bold;
        margin-bottom: .5rem;
      }

      &__close {
        font-size: 1.5rem;
        line-height: 1;
        padding: .5rem;
        cursor: pointer;

        position: absolute;
        top: 0;
        right: 0;
      }
    }

    // Filter Section
    // ---------------
    &-filter {
      border-top: 3px solid #2c3d45;
      padding: 1rem 0.7rem;
    }
  }
</style>
