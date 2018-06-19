<template lang="html">
  <div class="modal">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Add new card</p>
        <button class="delete" aria-label="close" @click='closeModal()'></button>
      </header>
      <section class="modal-card-body">
        <div class="container-form">
          <div class="field">
            <label class="label">Title</label>
            <div class="control">
              <input class="input" type="text" v-model="card.title" placeholder="Text input">
            </div>
          </div>
          <div class="field">
            <label class="label">Description</label>
            <div class="control">
              <input class="input" type="textarea" v-model="card.desc" placeholder="Text input">
            </div>
          </div>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button  @click='closeModal()' class="button">Cancel</button>
        <button class="button is-success" @click='addCard()'>Add card</button>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddCardModal',
  data() {
    return {
      card: {
        title: '',
        desc: '',
        tags: []
      }
    }
  },
  methods: {
    closeModal() {
      this.$emit('close-modal')
    },
    addCard() {
      this.$emit('add-card', this.card)
    }
  }
}
</script>

<style lang="sass">
  .modal
    display: flex !important
</style>
