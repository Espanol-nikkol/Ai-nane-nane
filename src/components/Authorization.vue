<template>
  <div class="auth">
    <button class="auth__btn btn" @click="onShowBtnClick">
      {{ btnText }}
    </button>
    <form v-if="showAuth" class="auth-form"  v-on:submit.prevent="onSubmit">
      <input
          class="auth-form__input"
          type="text"
          id="authUser"
          maxlength="30"
          required
          @input="onAuthInput"
          placeholder="Ваше имя">
      <button class="auth-form__btn btn" type="submit">
        Отправить
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: "Authorization",
  props: {
    showAuth: Boolean
  },
  methods: {
    onSubmit: function () {
      this.$emit("submit-auth")
    },
    onShowBtnClick: function () {
      this.$emit("btn-auth-click")
    },
    onAuthInput: function (evt) {
      if (
          this.$store.state.FORBIDDEN_SYMBOLS.test(evt.target.value)
      ) {
        evt.target.setCustomValidity("Пожалуйста, не используйте служебные символы")
      } else if (evt.target.value.includes("#")) {
        evt.target.setCustomValidity("Пожалуйста, без #")
      }
      else if (
          evt.target.value.trim() === ""
      ) {
        evt.target.setCustomValidity("Имя должно быть")
      } else {
        evt.target.setCustomValidity("")
      }
    },
  },
  computed: {
    btnText: function () {
      return this.$store.state.user === "Anonimus666" ? "Авторизация" : "Переавторизация"
    }
  }
}
</script>

<style>
  .auth {
    grid-row: 1/3;
    grid-column: 3;
  }

  .auth-form {
    padding-left: 50px;
  }

  .auth__btn {
    margin-bottom: 75px;
    font-weight: bold;
  }

  .auth-form__input {
    width: 170px;
    margin-bottom: 15px;
  }
</style>