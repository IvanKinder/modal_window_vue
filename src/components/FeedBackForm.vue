<template>
  <p>Заполни форму</p>
  <form>
    <fieldset>
      <legend>Контактная информация</legend>
      <p><label for="name">Имя <em>* </em></label><input v-model="userName" type="text" id="name"></p>
      <p><label for="email">E-mail  <em>* </em></label><input v-model="userEmail" type="email" id="email"></p>
    </fieldset>
  </form>
</template>

<script>
export default {
  name: "FeedBackForm",
  data () {
    return {
      userName: '',
      userEmail: ''
    }
  }
}
</script>

<style scoped>

</style>
