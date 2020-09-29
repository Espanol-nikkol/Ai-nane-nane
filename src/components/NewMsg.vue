<template>
  <form class="msg-new" v-on:submit.prevent="onSubmit">
    <input
        class="msg-new__input msg-new__input--room"
        v-if="!isChat"
        @input="onRoomInput"
        id="room-name"
        type="text"
        maxlength="34"
        required
        placeholder="Новый чат">
    <textarea class="msg-new__input msg-new__input--text" id="text-msg" required placeholder="Сообщение">
    </textarea>
    <input class="msg-new__input msg-new__input--sign" v-if="authorized" id="user" type="text" placeholder="Подпись">
    <button class="msg-new__btn btn" type="submit">
      Отправить
    </button>
  </form>
</template>

<script>

export default {
  name: "NewMsg",
  props: {
    isChat: Boolean
  },
  methods: {
    onSubmit: function () {
      this.$emit("submit-new-msg")
    },
    onRoomInput: function (evt) {
      if (
          this.$store.state.FORBIDDEN_SYMBOLS.test(evt.target.value) ||
          evt.target.value.includes("/")
      ) {
        evt.target.setCustomValidity("Пожалуйста, не используйте служебные символы")
      } else if (evt.target.value.includes("#")) {
        evt.target.setCustomValidity("Пожалуйста, без #")
      }
      else if (
          evt.target.value.trim() === "" ||
          evt.target.value === '.'
      ) {
        evt.target.setCustomValidity("У чата должно быть название")
      } else {
        evt.target.setCustomValidity("")
      }
    },
  },
  computed: {
    authorized: function () {
      return this.$store.state.user === "Anonimus666"
    }
  }
}
</script>

<style>
  .msg-new {
    display: grid;
    grid-template-columns: 826px 215px;
    grid-template-rows: repeat(2, 55px);
    grid-column: 2 / 3;
    grid-row: 3;

    position: relative;
    top: -20px;
  }

  .msg-new__input--room {
    height: 40px;

    position: absolute;
    top: -70px;

    grid-row: 1;
    grid-column: 1;
  }

  .msg-new__input--text {
    height: 100px;

    grid-column: 1;
    grid-row: 1/2;

    resize: none;
  }

  .msg-new__input--room, .msg-new__input--text {
    width: 790px;
  }

  .msg-new__btn {
    grid-column: 2;
    grid-row: 2;
  }

  .msg-new__input--sign {
    width: 165px;
    height: 25px;
    line-height: 45px;
    justify-self: center;

    grid-column: 2;
    grid-row: 1;
  }
</style>