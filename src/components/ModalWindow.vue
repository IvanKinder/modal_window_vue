<template>
  <div class="modal-wrapper"
       v-if="modalVisible"
  >
    <div class="modal">
      <div class="modal-header">
        <span>{{ modalName }}</span>
        <span>
        <button class="modal-close-btn"
                @click="closeModal"
        >
          Close</button>
      </span>
      </div>
      <hr>
      <div class="modal-content">
        <slot></slot>
      </div>
      <hr>
      <div class="modal-footer">
        <button
            v-for="(btn, index) in modalButtons"
            :key="index"
            @click="modalHandler(btn)"
            type="btn.type"
        >
          {{ btn.name }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalWindow',
  props: {
    modalName: {
      type: String,
      default: 'Warning'
    }
  },
  responseController: null,
  data() {
    return {
      modalVisible: false,
      modalButtons: [],
      clickedData: undefined
    }
  },
  methods: {
    show(modalButtons) {
      let resolve
      let reject
      const responsePromise = new Promise((ok, fail) => {
        resolve = ok
        reject = fail
      })

      this.modalButtons = modalButtons
      this.modalVisible = true

      this.$options.responseController = { resolve, reject }

      return responsePromise
    },
    closeModal() {
      this.modalVisible = false
      this.$options.responseController.resolve({
        data: null
      })
    },
    modalHandler(btn) {
      this.clickedData = btn
      this.$options.responseController.resolve({
        data: btn.handler()
      })
      this.modalVisible = false
    }
  }
}
</script>

<style scoped>
.modal-wrapper {
  width: 98vw;
  height: 98vh;
  position: absolute;
  top: 1vh;
  background-color: rgba(88, 94, 88, 0.6);
}

.modal {
  margin: 20vh auto;
  width: 40vw;
  padding: 20px;
  background-color: white;
  border-radius: 6px;
  box-shadow: 5px 5px 5px rgba(122, 122, 122, 0.5);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-footer {
  display: flex;
  justify-content: space-around;
}

.modal-content {
  max-height: 50vh;
  overflow: auto;
}
</style>
