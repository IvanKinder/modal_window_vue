<template>
  <div class="content">
    <button class="show-modal-btn" @click="showModalWindow">Show modal dialog</button>
    <ModalWindow modalName="Форма обратной связи" ref="modal">
      <FeedBackForm ref="feedbackform"/>
    </ModalWindow>
  </div>
</template>

<script>
import ModalWindow from './ModalWindow.vue'
import FeedBackForm from './FeedBackForm.vue'

export default {
  name: "ContentLayout",
  components: {
    ModalWindow,
    FeedBackForm
  },
  methods: {
    async showModalWindow () {
      const referenseItems = this.$refs
      const emailValidator = function (email) {
        const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        return re.test(email)
      }
      const nameValidator = function (name) {
        const re = /^(?=.{2,10}$)(?:[a-zA-Zа-яА-Я\d]+(?:(?:)[a-zA-Zа-яА-Я\d])*)+$/ui
        return re.test(name)
      }
      const modalButtons = [
        {
          name: 'Отмена',
          type: 'button',
          handler: function () {
            referenseItems.modal.closeModal()
          }
        },
        {
          name: 'Отправить',
          type: 'submit',
          handler: function () {
            if (!nameValidator(referenseItems.feedbackform.userName)) {
              return {
                status: 'Ошибка',
                message: 'Name is incorrect ( min 2 simbols, max 10 symbols )'
              }
            }
            if (!emailValidator(referenseItems.feedbackform.userEmail)) {
              return {
                status: 'Ошибка',
                message: 'Email is incorrect'
              }
            }
            return {
              status: 'Успех',
              name: referenseItems.feedbackform.userName,
              email: referenseItems.feedbackform.userEmail
            }
          }
        }
      ]
      const result = await referenseItems.modal.show(modalButtons)
      if (result.data) {
        alert(JSON.stringify(result.data))
      }
    }
  }
}
</script>

<style scoped>
.content {
  width: 98vw;
  height: 98vh;
  background-image: url('https://phonoteka.org/uploads/posts/2021-06/1624059795_5-phonoteka_org-p-oboi-les-krasivo-5.jpg');
}

.show-modal-btn {
  margin: 20px;
}
</style>
