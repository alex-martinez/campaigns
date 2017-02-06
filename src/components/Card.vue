<template>
  <section class="card">
    <div class="card-content" :contenteditable="isEditable">
      <div class="card-content__title" ref="campaignTitle">{{ title }}</div>

      <div class="card-content__description" ref="campaignDescription">{{ description }}</div>

      <div class="card-content__save" @click="saveContent()" v-if="isEditable">✓</div>
    </div>

    <footer class="card-footer">
      <div class="card-footer-item">{{ conversion }}%</div>

      <div class="card-footer-item">{{ id }}</div>

      <div class="card-footer-item">
        <div class="action-button" @click="toggleCardOverlay()"></div>
      </div>
    </footer>

    <div class="card-actions" v-if="showOverlay">
      <span class="card-actions__close" @click="toggleCardOverlay()">&times</span>
      <span href="#">Rules</span>
      <span @click="editCard()">Edit</span>
      <span @click="duplicateContent()">Duplicate</span>
      <span @click.prevent="deleteCard()">Delete</span>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'card',
    props: {
      editable: { type: Boolean },
      deleteCampaign: { type: Function, required: true },
      saveNewCampaign: { type: Function, required: true },
      saveEditedCampaign: { type: Function, required: true },
      duplicateCampaign: { type: Function, required: true },
      title: { type: String, required: true },
      description: { type: String, required: true },
      conversion: { type: Number, required: true },
      id: { type: String, required: true }
    },
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        showOverlay: false,
        isEditable: this.editable,
        isEditing: false, // Used to determine PUT or POST
        isNewCampaign: false // Used to determine PUT or POST
      }
    },
    methods: {
      toggleCardOverlay () {
        this.showOverlay = !this.showOverlay
      },
      editCard () {
        this.isEditable = !this.isEditable
        this.showOverlay = false
        this.isEditing = true
      },
      saveContent () {
        const campaignData = {
          title: this.$refs.campaignTitle.innerText,
          description: this.$refs.campaignDescription.innerText,
          conversion: 0,
          id: this.id
        }

        if (this.isEditing) {
          this.saveEditedCampaign(this.id, campaignData)
        } else {
          this.saveNewCampaign(campaignData)
        }

        // Reset states
        this.isNewCampaign = false
        this.isEditing = false
        this.isEditable = !this.isEditable
      },
      duplicateContent () {
        const makeId = () => {
          let id = ''
          const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'

          for (let i = 0; i < 7; i++) {
            id += chars.charAt(Math.floor(Math.random() * chars.length))
          }

          return id.toUpperCase()
        }

        const data = {
          title: `Copy ${this.title}`,
          description: this.description,
          conversion: 0,
          id: makeId()
        }

        this.duplicateCampaign(data) // Call prop method passed from parent
        this.toggleCardOverlay() // Hide overlay
      },
      deleteCard () {
        this.deleteCampaign(this.id) // Call prop method passed from parent
        this.toggleCardOverlay() // Hide overlay
      }
    }
  }
</script>


<style lang="scss" scoped>
  .action-button {
    $dimensions: 6px;
    $offset: 4px;
    $color: currentColor;
    $color-hover: #5d5d5d;

    cursor: pointer;
    position: relative;
    padding: 0.5rem ($offset + $dimensions + 10px);

    &:before,
    &:after {
      content: "";
      display: block;
    }

    // Outer circle
    &:before {
      width: $dimensions;
      height: $dimensions;
      border-radius: 50%;
      background-color: currentColor;
      box-shadow: ($offset + $dimensions) 0 0 currentColor,
                  -($offset + $dimensions) 0 0 currentColor;
    }

    &:hover:before {
      background-color: $color-hover;
      box-shadow: ($offset + $dimensions) 0 0 $color-hover,
                  -($offset + $dimensions) 0 0 $color-hover;
    }

    // Inner circle
    &:after {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: $dimensions / 2;
      height: $dimensions / 2;
      border-radius: 50%;
      background-color: #fff;
      box-shadow: ($offset + $dimensions) 0 0 #fff,
                  -($offset + $dimensions) 0 0 #fff;
    }
  }

  .card {
    color: #5d5d5d;
    position: relative;
    background-color: #fff;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1),
                0 0 0 1px rgba(10, 10, 10, 0.1);

    // Actions
    // ---------------
    &-actions {
      color: #fff;
      text-transform: uppercase;
      font-size: .9rem;

      background-color: #2ca2d0;
      padding: 1rem;

      display: flex;
      justify-content: center;
      align-items: center;

      // Overlay the card
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 10;

      span {
        cursor: pointer;
        display: inline-block;
        margin: 0 0.5rem;
      }

      &__close {
        position: absolute;
        top: 0;
        right: 0;

        font-size: 1.5rem;
        line-height: 1;
        text-decoration: none;
        padding: 5px;
        display: block;
        cursor: pointer;
      }
    }

    // Content
    // ---------------
    &-content {
      padding: 0.8rem 0.7rem;
      min-height: 100px;

      &[contenteditable="true"] {
        box-shadow: inset 0 0 0 1px #3273dc;
      }

      &__title {
        color: #5d5d5d;
        font-weight: bold;
      }

      &__description {
        color: #00d1b2;
      }

      &__save {
        position: absolute;
        top: 0;
        right: 0;

        background-color: #3273dc;
        color: #fff;

        font-size: 0.8rem;
        text-align: center;
        line-height: 1.3rem;

        width: 1.3rem;
        height: 1.3rem;

        cursor: pointer;
      }
    }

    // Footer
    // ---------------
    &-footer {
      color: #00d1b2;
      border-top: 1px solid #dbdbdb;
      display: flex;
      align-items: stretch;

      &-item {
        text-align: center;
        padding: 0.3rem 0.5rem;
        flex: 1 0 0;
        display: flex;
        justify-content: center;
        align-items: center;

        &:not(:last-child) {
          border-right: 1px solid #dbdbdb;
        }
      }
    }
  }
</style>
