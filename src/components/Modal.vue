<template>
    <div   div class="modal" :class="{'is-active': showModalFlag}">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Edit Item Modal</p>
          <button class="delete" aria-label="close" @click="cancelModal">></button>
        </header>
        <section class="modal-card-body">
          <input v-model="message" placeholder="edit me">
          <select v-model="selected" >
                <option value="verified">verified</option>
                <option value="unverified">unverified</option>
          </select>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success" @click="saveModal">Save</button>
          <button class="button" @click="cancelModal">Discard</button>
        </footer>
      </div>
    </div>
</template>
<script>
export default { 
   data() {
       return {
        showModalFlag: false,
        okPressed: false,
        message: "",
        selected: null,
        selectedId: null
       }
   },
   methods: {
    showModal(value) {
      this.okPressed = false;
      this.showModalFlag = true;
      this.message = value.name;
      this.selected = value.status;
      this.selectedId = value.id
    },
    saveModal() {
        this.okPressed = true;
        this.showModalFlag = false;
        this.$emit('save', { id:this.selectedId, newName: this.message, newStatus: this.selected});
    },
    cancelModal() {
        this.okPressed = false;
        this.showModalFlag = false;
    }
   }
  
}
</script>