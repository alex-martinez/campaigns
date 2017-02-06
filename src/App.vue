<template>
  <div id="app">
    <PageHeader
      :sidebarVisible="sidebarVisible"
      :toggleSidebar="toggleSidebar" />

    <Sidebar
      :sidebarVisible="sidebarVisible"
      :toggleSidebar="toggleSidebar"
      :orderCampaignsAscending="orderCampaignsAscending"
      :orderCampaignsDescending="orderCampaignsDescending"
      :sortCampaignsByName="sortCampaignsByName"
      :sortCampaignsByCreated="sortCampaignsByCreated" />

    <div class="card-wrapper">
      <div class="card-wrapper__item" v-for="campaign in campaigns" :key="campaign.id">
        <Card
          :editable="campaign.editable"
          :title="campaign.title"
          :description="campaign.description"
          :conversion="campaign.conversion"
          :id="campaign.id"
          :duplicateCampaign="duplicateCampaign"
          :deleteCampaign="deleteCampaign"
          :saveEditedCampaign="saveEditedCampaign"
          :saveNewCampaign="saveNewCampaign" />
      </div>
    </div>
  </div>
</template>

<script>
  import PageHeader from './components/PageHeader'
  import Card from './components/Card'
  import Sidebar from './components/Sidebar'
  import { API_CAMPAIGNS } from './constants'

  export default {
    name: 'app',
    components: {
      PageHeader,
      Card,
      Sidebar
    },
    mounted () {
      this.$http.get(API_CAMPAIGNS).then(resp => {
        this.campaigns = resp.body.reverse()
      })
    },
    data () {
      return {
        sidebarVisible: false,
        campaigns: []
      }
    },
    methods: {
      toggleSidebar () {
        this.sidebarVisible = !this.sidebarVisible
      },
      duplicateCampaign (data) {
        data.editable = true
        this.campaigns = [data, ...this.campaigns]
      },
      deleteCampaign (id) {
        this.campaigns = this.campaigns.filter(campaign => campaign.id !== id)
        this.$http.delete(`${API_CAMPAIGNS}/${id}`)
      },
      saveEditedCampaign (id, campaignData) {
        this.$http.put(`${API_CAMPAIGNS}/${id}`, campaignData)
      },
      saveNewCampaign (campaignData) {
        this.$http.post(`${API_CAMPAIGNS}`, campaignData)
      },
      orderCampaignsAscending () {
        this.campaigns.reverse()
      },
      orderCampaignsDescending () {
        this.campaigns.reverse()
      },
      sortCampaignsByName () {
        this.campaigns.sort((a, b) => {
          const titleA = a.title.toUpperCase()
          const titleB = b.title.toUpperCase()

          if (titleA > titleB) return -1

          if (titleA < titleB) return 1

          return 0
        })
      },
      sortCampaignsByCreated () {
        this.campaigns.sort((a, b) => {
          if (a.created > b.created) return -1

          if (a.created < b.created) return 1

          return 0
        })
      }
    }
  }
</script>

<style lang="scss">
  html {
    font-size: 16px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  body {
    font-size: 1rem;
    background-color: #eaeff3;
    margin: 0;
    padding-top: 75px; // fixed header height
  }

  *, *:before, *:after {
    box-sizing: border-box;
  }

  a {
    text-decoration: none;

    &:hover {
      text-decoration: underline;
    }
  }

  button {
    background-color: #6f8293;
    border-radius: 3px;
    border: none;
    padding: 0.5rem 1rem;
    font-weight: normal;
    color: #fff;
    font-size: 0.9rem;
    cursor: pointer;
  }

  .card-wrapper {
    padding: 0 .5rem;
    display: flex;
    flex-wrap: wrap;

    &__item {
      padding: .5rem;
      flex-basis: 100%;

      // Tablet
      @media only screen and (min-width: 768px) {
        flex-basis: 50%;
      }

      // Medium desktop/laptop
      @media only screen and (min-width: 1224px) {
        flex-basis: 33.33%;
      }

      // Large desktop
      @media only screen and (min-width: 1824px) {
        flex-basis: 25%;
      }
    }
  }
</style>
